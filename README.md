Dask Stories
============

This repository holds stories from experienced Dask users that are intended to
help new users get a sense for how Dask might apply to their field.

If you are curious about Dask then please read the various submitted stories.

If you use Dask today to solve interesting problems we would love to have you
share your story.  Hearing from experienced users like yourself can help
newcomers quickly identify the parts of Dask and the surrounding ecosystem that
are likely to be valuable to them.


## How to share your story

We welcome stories of any form.  However, if you'd like some guidance then we
recommend the following rubric.  We've included suggestions of how to break up
a story below, including two entirely fabricated stories alongside each set of
instructions.


### Who are you?

Include your name, the project you work with, and your role within that
project.  Some examples:

-  I'm [Joseph Chen]().  I manage a quantitative quality control group at
   [XYZ automotive](), a company that makes automotive parts.
-  I'm [Alice Singh](), I'm a post-doctoral scholar at the University of
   Arizona.  I work at the [National Solar Observatory]() studying sunspots and
   solar flare.

Links are welcome.


### What problem are you trying to solve?

Include context and detail here about the problem that you're trying to solve.
Details are *very* welcome here.  You're probably writing to someone within
your own field so feel free to use technical speech.  You shouldn't necessarily
mention Dask here; focus on your problem instead.

#### Example XYZ Automotive

XYZ automomotive produces thousands of kinds of parts for cars and millions of
each part.  Many of these parts generate telemetry about how they're doing on a
second-by-second basis.  We process this information over time to learn when
parts might fail, and what kinds of activities might lead to failure.  This
helps us react in a variety of ways:

1.  We signal drivers that they should service their vehicle soon, reducing the
    chance of a serious problem while on the road
2.  We inform automotive mechanics where the problem is, reducing the cost of
    diagnostics
3.  We roll these discoveries back into the design process, helping our
    engineers develop solid products.

However, analyzing billions of time series is hard, especially when those time
series come from thousands of different kinds of devices.  This is both a "big
data" problem and a "heterogeneous data" problem.  We employ a team of data
scientists to analyze this data.  We use Pandas, Scikit-Learn, statsmodels, and
[lifelines](http://lifelines.readthedocs.io/en/latest/) for survival analysis,
along with some internal libraries.  In particular we found that scaling
survival analysis to be as our business grew.

#### Example: Solar Astronomers

My research analyzes correlation information from high-resolution solar
astronomy data that might preceed solar flare activity or correlate with
sunspots.  This both helps our understanding of basic science in
Magento-Hydro-Dynamics (MHD) as well as improves the durability of Earth's
satellite fleet during dangerous solar storm events.

In practice this means that we build algorithms to analyze a real-time stream
of high-resolution images of the sun to predict future activity.  We do image
segmentation to find spots for testing data, and discrete wavelet transforms
*both spatially and across time* to create featrures for downstream machine
learning algorithms.

Our data is big.  Single images can be hundreds of megabytes and we have years
of them.  This means hundreds of terabytes.


### How Dask Helps

Describe how Dask helps to solve this problem.  Again, details are welcome.
New readers probably won't know about specific API like "we use client.scatter"
but probably will be able to follow terms used as headers in documentation like
"we used dask dataframe and the futures interface together".

We also encourage you to mention how your use of Dask has *changed* over time.
What originally drew you to the project?  Is that still why you use it or has
your perception or needs changed?

#### Example: XYZ Automotive

Dask helps us solve our problem by parallelizing our internal timeseries
libraries.  We're in a weird position where we have "big data", but it's all
very different, so standard projects like time-series databases, Apache Spark,
or Dask Dataframe weren't really a good fit.  However, we found that we could
just use Pandas/Scikit-Learn/Lifelines code along with [Dask delayed]() to
parallelize our existing solutions pretty easily.  The code is still pretty
much what we had before (which is good, we had a lot invested there) but now it
operates well on larger datasets and using all of our cores.

As a result of this our large analysis workstations quickly became very
popular and we've had to acquire more, but people seem pretty happy.  We've
looked into the distributed scheduler for cluster computing and it's
interesting, but we haven't found a sufficient business need yet.

#### Example: Solar Astronomer

We use [Dask Arrays]() to process large stacks of images with the Numpy API.
I and most of my colleagues are comfortable with Numpy, and so the learning
curve to parallelize with Dask Array was very easy.  We have two common
workloads:

-  We combine Dask arrays with the Scikit-Image library to apply standard
   filters over image stacks in an embarrassingly parallel way.  Nothing fancy
   here, but it's nice not to have to worry about saturating cores and keeping
   memory use low.  Dask handles that for us.
-  We rechunk our images to be in blocks that include time, then we overlap
   them with halos so that neighboring blocks have a bit of nearby information,
   then we apply more complex algorithms, mostly DWT, but we're also starting
   to experiment with convolutional neural nets

Finally, we also just use Dask array as we play around with new datasets.  It's
our go-to solution now for just hacking around on our laptop.

We're also starting to use Dask's futures interface while we prototype out a
real-time processing system for live analysis and alerts, but that's a separate
project.

Originally most of this work started on my laptop with the local scheduler.
However as we've started doing some more complex workloads we've found that
Dask is no longer able to do everything in low memory.  As a result we now run
dask.distributed on our cluster on 10+TB image stacks.  We've thought about
going bigger, but don't yet have the allocation.


### Some of the pain points of using Dask for your problem

Dask has issues and it's not always the right solution for every problem.  What
are things that you ran into that you think others in your field should know
ahead of time?


#### Example: XYZ Automotive

When we started using parallelism we found that large sections of our codebase
didn't parallelize well.  We used a lot of string comparisons that didn't play
well with the GIL.  We switched to uses Dask's multiprocessing scheduler but
then communication costs were too high.  Eventually we worked around this by
using Pandas categoricals, which solves the problem pretty well, but only up to
about 16 cores per process.

We also have some problems with diagnostics.  The diagnostics on the local
scheduler aren't as nice as those on the distributed scheduler.  There are
tradeoffs both ways


#### Example: Solar Astronomer

Getting new users comfortable with lazy evaluation took a bit of time.

When we first started the overhead to overlapping computations with dask array
was really high.  It looks like this has been mostly fixed in the recent
version though.  Still though, as we continue to scale out to larger datasets
we feel like we always run into new kinds of overhead.  It can be worked around
and things are getting a lot better, but if you want to go to 100TB expect to
do some tuning.  Fortunately the codebase is all Python, so we've been able to
improve things and send patches upstream.

When using the distributed scheduler we had problems with our HPC cluster,
which is pretty finicky about workers running out of RAM.  It took a bit to get
configuration right so that workers killed themselves before the cluster found
out about them.


### Some of the technology you use around Dask

This might be other libraries that you use with Dask for analysis or data
storage.  Cluster technologies that you use to deploy or capture logs, etc..
Anything that you think someone like you would like to know about.


#### Example: XYZ Automotive

We mostly use Pandas, Scikit-Learn, and Lifelines for computation.  We get data
in CSV and convert to Parquet using Arrow.  We use the general PyData stack for
plotting and such.


#### Example: Solar Astronomer

We store our data in FITS files and use AstroPy to read it, but now we're
looking at moving over to TIFF or Zarr.  We've also just started looking at
XArray, which has good Dask support and seems to have a strong community.

For cluster deployment we use PBS and the
[dask-jobqueue](https://dask-jobqueue.readthedocs.io) project locally, though
we're starting to look at storing data on AWS and using the [Dask-Helm
chart](http://dask.pydata.org/en/latest/setup/kubernetes-helm.html) or
[dask-kubernetes](https://dask-kubernetes.readthedocs.io)
