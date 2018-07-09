### Who am I?

I am [Joe Hamman](http://joehamman.com/about/) and I am a Project Scientist in the [Computational Hydrology Group](https://ncar.github.io/hydrology/) at the [National Center for Atmospheric Research](https://ncar.ucar.edu/). I am a core developer of the [xarray](http://xarray.pydata.org) project and a contributing member of the [Pangeo](http://pangeo-data.org/) project. I study subjects in the areas of climate change, hydrology, and water resource engineering.

### What problem am I trying to solve?

Climate change will bring widespread impacts to the hydrologic cycle. We know this because many research studies, conducted over the past two decades, have shown what the first order effects of climate change will look like in managed and natural hydrologic systems in terms of things like water availability, drought, wildfire, extreme precipitation, and floods. However, we don't have a very good understanding of the characteristic uncertainties that come from our choice of tools that we use to estimate these changes. In the field of hydroclimatolgy, the tools we use are numerical models of the climate and hydrologic systems. These models can be constructed in many ways and it is often difficult understand how specific choices we make when building a model impact the inferences we can draw from them (e.g. the impact of climate change on flood frequency).  We are working on methods to expose and constrain methodological uncertainties in the climate impacts modeling paradigm for water resource applications. This includes developing and analyzing large ensembles of climate projections and interrogating these ensembles to understand specific sources of uncertainty.

### How does Dask help?

The climate and hydrologic sciences rely heavily on storage formats like HDF5 and NetCDF. We often use [xarray](http://xarray.pydata.org) as an interface and friendly data model for these formats. Under the hood, xarray uses either NumPy or Dask arrays. This allows us to scale the same xarray computations we would typically do in-memory using NumPy, to larger tasks using Dask.

In my own research, I use Dask and xarray as the core computational libraries for working with large datasets (10s-100s terabytes). Often times the operations we do with these datasets are fairly simple reduction operations where we may compare the average climate from two periods.

### Why did you choose Dask?

When working on scientific analysis tasks, I don't want to think about parallelizing my code. We've chosen to work with Dask because `Dask.array`'s nearly drop-in compatible API with NumPy meant adopting Dask, inside or outside of xarray, was much easier than adopting another parallel framework. Along those same lines, Dask is well integrated with other key parts of the scientific Python stack, including Pandas, Sklearn, etc.

### Pain points?

- Deploying (has gotten much easier)
- Its easy to use (but also easy to break)
- Diagnosing performance issues is still an art, not a science

### Technology around Dask?

- Xarray provides a high level (and familiar) interface
- I mostly work on HPC systems and have been helping develop the dask-jobqueue package for deploying dask on job queueing systems
- In the Pangeo project, we're exploring dask applications using kubernetes and jupyter notebooks

### Anything else to know?

I'm quite interested in enabling more intuitive scientific analysis workflows, particularly when parallelization is required. Dask has been a big part of our efforts to facilitate a "begining-to-end" workflow pattern on large datasets.
