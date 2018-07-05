### Who am I?

I'm [James Bourbeau](https://github.com/jrbourbeau). I'm a graduate student in
the Physics department at the University of Wisconsin&mdash;Madison and a member
of the [IceCube South Pole Neutrino Observatory Collaboration](https://icecube.wisc.edu/).


### What problem am I trying to solve?

Cosmic rays are energetic particles that originate from outer space. While they
have been studied since the early 1900s, the sources of high-energy cosmic rays
are still not well known. I use data collected by IceCube to study how the
cosmic-ray flux changes with energy and particle mass. This can help provide
valuable insight into our understanding of the origin of cosmic rays.


### How does Dask help?

My analysis pipeline consists of several steps: data preprocessing,
training/validating machine learning models, performing statistical analyses,
implementing custom algorithms, etc. Dask provides a user-friendly framework for
both parallelizing each of these steps as well as seamlessly scaling to large
amounts of data when required.


### Why did you choose Dask?

I originally chose to use Dask because of the built-in dask.array and
dask.dataframe data structures. Since I was already using NumPy and pandas, Dask
served effectively as a drop-in replacement for these libraries that allowed my
code to scale to larger datasets. Later I discovered dask.delayed and now use it
frequently to parallelize code that doesn't easily conform to the dask.array /
dask.dataframe use case.


### Pain points?

There were two main pain points I encountered when first using Dask:

- Getting used to the idea of lazy computation. While this isn't an issue that is
specific to Dask, it was something that took some time to get used to.

- Dask is a fairly large project with many components and it took me some time to
figure out how all the pieces fit together. Luckily, the user documentation for
Dask is quite good and I was able to get over this initial learning curve.


### Technology around Dask?

- HTCondor
- scikit-learn&mdash;the development currently going on with Dask-ml is really
exciting.


### Anything else to know?

Dask has become my go-to tool for parallelizing code and working with large
amounts of data. It's worth mentioning that Dask offers several diagnostic tools
such as a progress bar, memory and CPU profilers, etc. These aren't strictly
necessary for my work, but they are very nice to have.
