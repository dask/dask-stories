Satellite Imagery Processing
============================

Who am I?
---------

I am [David Hoese](http://github.com/djhoese) and I work as a software
developer at the [Space Science and Engineering Center
(SSEC)](https://www.ssec.wisc.edu/) at the University of Wisconsin - Madison.
My job is to create software that makes meteorological data more accessible to
scientists and researchers.

I am also a member of the open source
[PyTroll](http://pytroll.github.io/) community where I act as a core developer
on the [SatPy](http://satpy.readthedocs.io/en/latest/) library. I use SatPy in
my SSEC projects called Polar2Grid and Geo2Grid which provide a simple command
line interface on top of the features provided by SatPy.


The Problem I'm trying to solve
-------------------------------

Satellite imagery data is often hard to read and use because of the many
different formats and structures that it can come in. To make satellite imagery
more useful the SatPy library wraps common operations performed on satellite
data in simple interfaces. Typically meteorological satellite data needs to go
through some or all of the following steps:

- **Read:** Read the observed scientific data from one or more data files while
    keeping track of geolocation and other metadata to make the data the
    most useful and descriptive.
- **Composite:** Combine one or more different "channels" of data to bring out
    certain features in the data. This is typically shown as RGB
    images.
- **Correct:** Sometimes data has artifacts, from the instrument hardware or the
    atmosphere for example, that can be removed or adjusted.
- **Resample:** Visualization tools often support a small subset of Earth
    projections and satellite data is usually not in those
    projections. Resampling can also be useful when wanting to do
    intercomparisons between different instruments (on the same
    satellite or not).
- **Enhancement:** Data can be normalized or scaled in certain ways that makes
    certain atmospheric conditions more apparent. This can also
    be used to better fit data in to other data types (8-bit
    integers, etc).
- **Write:** Visualization tools typically only support a few specific file
    formats. Some of these formats are difficult to write or have small
    differences depending on what application they are destined for.

As satellite instrument technology advances, scientists have to learn how to
handle more channels for each instrument and at spatial and temporal
resolutions that were unheard of when they were learning how to use satellite
data. If they are lucky, scientists may have access to a high performance
computing system, while the rest may have to settle for long execution times on
their desktop or laptop machines. By optimizing the parts of the processing
that take a lot of time and memory it is our hope that scientists can worry
about the science and leave the annoying parts to SatPy.


How Dask helps
--------------

SatPy's use of Dask makes it possible to do calculations on laptops that
used to require high performance server machines. SatPy was originally
drawn to [Xarray](http://xarray.pydata.org/en/stable/)'s `DataArray` objects
for the metadata storing functionality and the support for Dask arrays. We knew
that our original usage of Numpy masked arrays was not scalable to the new
satellite data being produced.  SatPy has now switched to `DataArray`
objects backed by Dask and leverages Dask's ability to do the following:

- **Lazy evaluation:** Software development is so much easier when you don't have
    to remove intermediate results from memory to process the next step.
- **Task caching:** Our processing involves a lot of intermediate results that can
    be shared between different processes. When things are optimized in the
    Dask graph it saves us as developers from having to code the "reuse" logic
    ourselves. It also means that intermediate results that are no longer
    needed can be disposed of and their memory freed.
- **Parallel workers and array chunking:** Satellite data is usually compared by
    geographic location. So a pixel at one index is often compared with the
    pixel of another array at that same index. Splitting arrays in to chunks
    and processing them separately provides us with a great performance
    improvement and not having to manage which worker gets what chunk of the
    array makes development effortless.

Benefiting from all of the above lets us create amazing high resolution RGB
images in 6-8 minutes on 3 year old laptops that would have taken 35+ minutes
to crash from memory limitations with SatPy's old Numpy implementation.


Pain points when using Dask
---------------------------

1.  Dask arrays are not Numpy arrays. Almost everything is supported or is
    close enough that you get used to it, but not everything. Most things you
    can get away with and get perfectly good performance; others you may end up
    computing your arrays multiple times in just a couple lines of code when
    you didn't know it.  Sometimes I wish that there was a Dask feature to
    raise an exception of your array is computed without you specifically
    saying it was ok.

2.  Writing to common satellite data formats, like GeoTIFF, can't always be
    written to by multiple writers (multiple nodes on a cluster) and some
    aren't even thread-safe. Opening a file object and using it with
    `dask.array.store` may work with some schedulers and not others.

3.  Dimension changes are a pain. Satellite data processing some times involves
    lookup tables to save on bandwidth limitations when sending data from the
    satellite to the ground or other similar situations. Having to use lookup
    tables, including something like a KDTree, can be really difficult and
    confusing to code with Dask and get it right. It typically involves using
    `atop`, `map_blocks`, or sometimes suffering the penalty of passing things
    to a `Delayed` function where the entire data array is passed as one
    complete memory-hungry array.

4.  A lot of satellite processing seems to perform better with the default
    threaded Dask scheduler over the distributed scheduler due to the nature of
    the problems being solved. A lot of processing, especially the creation of
    RGB images, requires comparing multiple arrays in different ways and can
    suffer from the amount of communication between distributed workers. There
    isn't an easy way that I know of to control where things are processed and
    which scheduler to use without requiring users to know detailed information
    on the internal of Dask.


Technology I use around Dask
----------------------------

As mentioned earlier SatPy uses [Xarray](http://xarray.pydata.org/en/stable/)to
wrap most of our Dask operations when possible. We have other useful tools that
we've created in the PyTroll community to help support deploying satellite
processing tools on servers, but they are not specific to Dask.


Links
-----

- [PyTroll Community](http://pytroll.github.io/)
- [SatPy](http://satpy.readthedocs.io/en/latest/)
- [SatPy Examples](http://satpy.readthedocs.io/en/latest/examples.html)
- [PyResample](http://pyresample.readthedocs.io/en/latest/)
