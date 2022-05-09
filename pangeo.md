Pangeo: Earth Science
=====================

Copyright and License
---------------------

Copyright 2020 Ryan Abernathey. I license this work under a [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) license.


Who Am I?
---------

I am [Ryan Abernathey](http://rabernat.github.io), a physical oceanographer and
professor at [Columbia University](http://columbia.edu) /
[Lamont Doherty Earth Observatory](http://ldeo.columbia.edu).

I am a founding member of the [Pangeo Project](http://pangeo.io), an
initiative aimed at coordinating and supporting the development of open source
software for the analysis of very large geoscientific datasets such as
satellite observations or climate simulation outputs.  Pangeo is funded by
[National Science Foundation Grant
1740648](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1740648&HistoricalAwards=false),
of which I am the principal investigator.

What Problem are We Trying to Solve?
------------------------------------

Many oceanographic and atmospheric science datasets consist of multi-dimensional
arrays of numerical data, such as temperature sampled on a regular latitude,
longitude, depth, time grid. These can be real data, observed by instruments
like weather balloons, satellites, or other sensors; or they can be "virtual"
data, produced by simulations. Scientists in these fields perform an extremely
wide range of different analyses on these datasets. For example:

- simple statistics like mean and standard deviation
- principal component analysis of spatio-temporal variability
- intercomparison of datasets with different spatio-temporal sampling
- spectral analysis (Fourier transforms) over various space and time dimensions
- budget diagnostics (e.g. calculating terms in the equation for heat conservation)
- machine learning for pattern recognition and prediction

Scientists like to work interactively and iteratively, trying out calculations,
visualizing the results, and tweaking their code until they eventually settle on
a result that is worthy of publication.

The traditional workflow is to download datasets to a personal laptop or
workstation and peform all analysis there. As sensor technology and computer
power continue to develop, the volume of our datasets is growing exponentially.
This workflow is not feasible or efficient with multi-terabyte datasets, and it
is impossible with petabyte-scale datasets. The fundamental problem we are
trying to solve in Pangeo is **how do we maintain the ability to perform
rapid, interactive analysis in the face of extremely large datasets?**
Dask is an essential part of our solution.

How Dask Helps
--------------

Our large multi-dimensional arrays map very well to Dask's `array` model. Our
users tend to interact with Dask via [Xarray](http://xarray.pydata.org), which
adds additional label-aware operations and group-by / resample capabilities.
The Xarray data model is explicitly inspired by the Common Data Model format
widely used in geosciences. Xarray has incorporated dask from very early in its
development, leading to close integration between these packages.

Pangeo provides configurations for deploying Jupyter, Xarray and Dask on
high-performance computing clusters and cloud platforms. On these platforms,
our users load data lazily using xarray from a variety of different storage
formats and perform analysis inside Jupyter notebooks. Working closely with
the Dask development team, we have tried to simplify the process of launching
Dask clusters interactively by using packages such as
[dask-kubernetes](https://github.com/dask/dask-kubernetes) and
[dask-jobqueue](https://github.com/dask/dask-jobqueue).
Users employ those packages to interactively launch their own Dask clusters
across many nodes of the compute system. Dask then automatically parallelizes
the xarray-based computations without users having to write much specialized
parallel code. Users appreciate the Dask dashboard, which provides a visual
indication of the progress and efficiency of their ongoing analysis. When
everything is working well, Dask is largely transparent to the user.

Why We Chose Dask Originally
----------------------------

Pangeo emerged from the Xarray development group, so Dask was a natural choice.
Beyond this, Dask's flexibility is a good fit for our applications; as
described above, scientists in this domain perform a huge range of different
types of analysis. We need a parallel computing engine which does not strongly
constrain the type of computations that can be performed nor require the user
to engage with the details of parallelization.

Pain Points
-----------

Dask's flexibility comes with some overhead.
I have the impression that the size of the graphs our users generate, which
can easily exceed a million tasks, is pushing the limits of the dask scheduler.
It is not uncommon for the scheduler to crash, or to take an uncomfortably long
time to process, when these tasks are submitted. Our workaround is mostly to
fall back on the sort of loop-based iteration over large datasets that we had
to do pre-Dask. All of this undermines the interactive experience we are trying
to achieve.

However, the first year of this project has made me optimistic about the future.
I think the interaction between Pangeo users and Dask developers has been
pretty successful. Our use cases have helped identify several performance
bottlenecks that have been fixed at the Dask level. If this trend can continue,
I'm confident we will be able to reach our desired scale (petabytes) and speed.

A broader issue relates to onboarding of new users. While I said above that
Dask operates transparently to the users, this is not always the case. Users
used to writing loop-based code to process datasets have to be retrained around
the delayed-evaluation paradigm. It can be a challenge to translate legacy code
into a Dask-friendly format. Some sort of "cheat sheet" might be able to help
with this.

Technology around Dask
----------------------

[Xarray](https://xarray.pydata.org) is the main way we interact with Dask. We use the
[`dask-jobqueque`](https://jobqueue.dask.org) and
[`dask-kubernetes`](https://kubernetes.dask.org) projects heavily.

We also use [Zarr](http://zarr.readthedocs.io) extensively for storage,
especially on the cloud, where we also employ
[`gcsfs`](https://gcsfs.readthedocs.io) and
[`s3fs`](https://s3fs.readthedocs.io) to interface with cloud storage.

