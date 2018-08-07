Satellite Imagery Processing
============================

Who am I?
---------

I am [David Hoese](http://github.com/djhoese) and I work as a software
developer at the Space Science and Engineering Center (SSEC) at the
University of Wisconsin - Madison. In a nut shell my job is to create
software that makes meteorological data more accessible to scientists and
researchers. I am also a member of the open source
[PyTroll](http://pytroll.github.io/) community where I act as a core
developer on the SatPy library. I use SatPy in my SSEC projects called
Polar2Grid and Geo2Grid which provide a simple command line interface on top
of the features provided by SatPy.


The Problem I'm trying to solve
-------------------------------

Satellite imagery data comes in many shapes, sizes, and formats. Atmospheric
satellite data typically comes from two types of satellites: geostationary and
polar-orbitting. Each satellite typically has multiple instruments or sensors
for observing the earth in different ways and each instrument usually measures
different channels or wavelengths of electromagnetic radiation emitted from
or reflected off the earth. The files that these data are provided in depend
greatly on what the instrument is measuring, which organization is running the
satellite mission, who is creating the files, and even what command line flags
were used when creating the files. This makes loading data from these files
very difficult.

Loading data from files is just the beginning of a typical satellite
processing workflow. The data may need to be calibrate or corrected to remove
certain affects from the instrument, sun, or atmosphere and be more useful for
a particular type of analysis. To be viewable in various visualization tools
the data must also be projected or resampled to a certain model and shape of
the earth. To be viewed as images the data is usually scaled in specific ways
to bring out certain features of the data including making RGB color images by
combining multiple channels of the data. Lastly, the data usually has to be
written to another file format to be used by a scientist's target audience
whether it be other scientists, forecasters, or the general public. Each group
has certain tools and certain expectations about what they should be able to
do with the data. As satellite instrument technology advances scientists are
now having to learn how to handle more channels for each instrument and at
spatial and temporal resolutions that were unheard of when they were learning
how to use satellite data. If they are lucky scientists may have access to a
high performance computing system, while the rest may have to settle for long
execution times on their desktop or laptop machines.

SatPy tries to make all of this processing easy to handle with simple
interfaces and logical defaults to produce the best results. By optimizing the
parts of the processing that typically take a lot of time and memory it is our
hope that scientists can worry about the science and leave the annoying parts
to SatPy.


How Dask helps
--------------

SatPy was originally drawn to dask for its lazy evaluation and multi-threaded
processing. Dask is used as the array backend for the xarray library's
`DataArray` object which is used as the main data container object in SatPy.
DataArray objects allow SatPy to keep the satellite imagery data, data
coordinates, and other metadata all in one place. Before using xarray and dask
SatPy used numpy masked arrays but as new satellite instruments were launched
our users simply couldn't handle the amount of memory required to perform
basic operations. The processing involved in SatPy requires a lot of
intermediate variables/calculations and comparisons between different data
arrays. With masked arrays and the right calculations you can quickly run out
of memory on a 16-32GB machine with instruments launched in the last 5-10
years.

Dask allows SatPy to split arrays in to multiple chunks and process them in
parallel, something that is extremely useful in satellite processing where
pixels at a particular array index are all processed the same or compared with
each other. By using `NaN` as a fill value in our dask arrays instead of having
a separate mask array we reduced our memory consumption a ton. And due to the
way dask schedules and caches tasks SatPy is able to reduce the amount of
intermediate calculation results stored in memory at any given time. This
makes it possible to do calculations on laptops that used to require high
performance server machines.

Lastly, developing software for satellite processing used to take a long time
because failing at the end of the processing chain sometimes required
reprocessing all of the previous steps, filling up memory, and then releasing
it to start the next step of processing. With dask we can persist results in
memory if we want or at the very least hold on to the dask graph for the steps
already completed.


Pain points when using dask
---------------------------

1. Dask is not numpy. Almost everything is supported or is close enough that
you get used to it, but not everything. Most things you can get away with and
get perfectly good performance; others you may end up computing your arrays
multiple times in just a couple lines of code when you didn't know it.

2. Writing to common satellite data formats, like geotiff, can't always be
written to by multiple writers (multiple nodes on a cluster) and some aren't
even thread-safe. Opening a file object and using it with `dask.array.store`
may work with some schedulers and not others.

3. Dimension changes are a pain. Satellite data processing some times involves
lookup tables to save on bandwidth limitations when sending data from the
satellite to the ground or other similar situations. Having to use lookup tables,
including something like a KDTree, can be really difficult and confusing to code
with dask and get it right. It typically involves using `atop`, `map_blocks`, or
sometimes suffering the penalty of passing things to a `Delayed` function where
the entire data array is passed as one complete memory-hungry array.

4. A lot of satellite processing seems to perform better with the default
threaded dask scheduler over the distributed scheduler due to the nature of
the problems being solved. A lot of processing, especially the creation of
RGB images, requires comparing multiple arrays in different ways and can
suffer from the amount of communication between distributed workers. There
isn't an easy way that I know of to control where things are processed and
which scheduler to use without requiring users to know detailed information
on the internal of dask.

Technology I use around Dask
----------------------------

As mentioned earlier SatPy uses xarray to wrap most of our dask operations
when possible. We have other useful tools that we've created in the PyTroll
community to help support deploying satellite processing tools on servers,
but they are not specific to dask.


Links
-----

- [PyTroll Community](http://pytroll.github.io/)
- [SatPy](http://satpy.readthedocs.io/en/latest/)
- [SatPy Examples](http://satpy.readthedocs.io/en/latest/examples.html)
- [PyResample](http://pyresample.readthedocs.io/en/latest/)
