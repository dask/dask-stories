### Who am I?

I am [Joe Hamman](http://joehamman.com/about/) and I am a Project Scientist in the [Computational Hydrology Group](https://ncar.github.io/hydrology/) at the [National Center for Atmospheric Research](https://ncar.ucar.edu/). I am a core developer of the [Xarray](http://xarray.pydata.org) project and a contributing member of the [Pangeo](http://pangeo-data.org/) project. I study subjects in the areas of climate change, hydrology, and water resource engineering.

### What problem am I trying to solve?

Climate change will bring widespread changes to the hydrologic cycle. While previous research has shown what the first order impacts of climate change will look like in managed and natural hydrologic systems, we don't have a very good understanding of the characteristic uncertainties that come from our choice of tools (climate and hydrologic models). We are working on methods to expose and constrain methodological uncertainties in the climate impacts modeling paradigm for water resource applications. This includes developing and analyzing large ensembles of climate projections.

### How does Dask help?

The climate and hydrologic sciences rely heavily on storage formats like HDF5 and NetCDF. We often use [Xarray](http://xarray.pydata.org) as an interface and friendly data model for these formats. Dask allows us to scale the same computations we do in-memory using Xarray and NumPy. Because using Dask with Xarray is mostly invisible to the user, Dask allows us, as scientists, to focus on the science part of our job, without having to rewrite our software for a completely different parallel computing framework.

### Why did you choose Dask?

When working on scientific analysis tasks, I don't want to think about parallelizing my code. We've chosen to work with Dask because `Dask.array`'s nearly drop-in compatible API with NumPy meant adopting Dask, inside or outside of Xarray, was much easier than adopting another parallel framework. Along those same lines, Dask is well integrated with other key parts of the scientific Python stack, including Pandas, Sklearn, etc.

### Pain points?

- Deploying (has gotten much easier)
- Its easy to use (but also easy to break)
- Diagnosing performance issues is still an art, not a science

### Technology around Dask?

- Xarray provides a high level (and familiar) interface
- I mostly work on HPC systems and have been helping develop the dask-jobqueue package for deploying dask on job queueing systems
- In the Pangeo project, we're exploring dask applications using kubernetes and jupyter notebooks.
-

### Anything else to know?

I'm quite interested in enabling more intuitive scientific analysis workflows, particularly when parallelization is required. Dask has been a big part of our efforts to facilitate a "begining-to-end" workflow pattern on large datasets.
