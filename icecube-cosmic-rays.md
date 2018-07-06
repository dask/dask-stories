### Who am I?

I'm [James Bourbeau](https://github.com/jrbourbeau), I'm a graduate student in
the Physics department at the University of Wisconsin&mdash;Madison. I work at
the [IceCube South Pole Neutrino Observatory](https://icecube.wisc.edu/)
studying the cosmic-ray energy spectrum.


### What problem am I trying to solve?

Cosmic rays are energetic particles that originate from outer space. While they
have been studied since the early 1900s, the sources of high-energy cosmic rays
are still not well known. I analyze data collected by IceCube to study how the
cosmic-ray spectrum changes with energy and particle mass; this can help provide
valuable insight into our understanding of the origin of cosmic rays.

This involves developing algorithms to perform energy reconstruction as well as particle mass group classification for events detected by IceCube. In addition, we use detector simulation and an iterative unfolding algorithm to correct for inherit detector biases and the finite resolution of our reconstructions.


### How Dask Helps

I originally chose to use Dask because of the [Dask Array](http://dask.pydata.org/en/latest/array.html) and
[Dask Dataframe](http://dask.pydata.org/en/latest/dataframe.html) data structures. I use Dask Dataframe to load thousands of [HDF](https://www.hdfgroup.org/) files and then apply further feature engineering and filtering data preprocessing steps. The final dataset can be up to 100GB in size, which is too large to load into our available RAM. So being able to easily distribute this load while still using the familiar pandas API has become invaluable in my research.

Later I discovered the [Dask delayed](http://dask.pydata.org/en/latest/delayed.html) iterface and now use it to parallelize code that doesn't easily conform to the Dask Array or Dask Dataframe use cases. For example, I often need to perform thousands of independent calculations for the pixels in a HEALPix sky map. I've found Dask delayed to be really useful for parallelizing these types of embarrassingly parallel calculations with minimal hassle.

I also use several of the [diagnostic tools](http://dask.pydata.org/en/latest/diagnostics-local.html) Dask offers such as the progress bar and resource profiler. Working in a large collaboration with shared computing resources, it's great to be able to monitor how many resources I'm using and scale back or scale up accordingly.


### Some of the pain points of using Dask for your problem

There were two main pain points I encountered when first using Dask:

- Getting used to the idea of lazy computation. While this isn't an issue that is
specific to Dask, it was something that took time to get used to.

- Dask is a fairly large project with many components and it took some time to
figure out how all the various pieces fit together. Luckily, the user documentation for
Dask is quite good and I was able to get over this initial learning curve.


### Some of the technology that we use around Dask

We store our data in HDF files, which Dask has nice read and write support for. We also use several other Python data stack tools like Jupyter, scikit-learn, matplotlib, seaborn, etc. Recently, we've started experimenting with using HTCondor and the [Dask distributed scheduler](https://distributed.readthedocs.io/en/latest/) to scale up to using hundreds of workers on a cluster.
