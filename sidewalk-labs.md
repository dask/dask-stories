### Who am I?

I'm [Brett Naul](https://github.com/bnaul).
I work at [Sidewalk Labs](https://www.sidewalklabs.com/).


### What problem am I trying to solve?

My team @ Sidewalk ("Model Lab") uses machine learning models to study human
travel behavior in cities and produce high-fidelity simulations of the travel
patterns/volumes in a metro area. Our process has three main steps:

-   Construct a "synthetic population" from census data and other sources of
    demographic information; this population is statistically representative of
    the true population but contains no actual identifiable individuals.

-   Train machine learning models on anonymous mobile location data to
    understand behavioral patterns in the region (what times do people go to
    lunch, what factors affect an individual's likelihood to use public
    transit, etc.).

-   For each person in the synthetic population, generate predictions from
    these models and combine the resulting into activities into a single
    model of all the activity in a region.


### How Dask Helps

Generating activities for millions of synthetic individuals is extremely
computationally intensive; even with for example, a 96 core instance,
simulating a single day in a large region initially took days. It was important
to us to be able to run a new simulation from scratch overnight, and scaling to
hundreds/thousands of cores across many workers with Dask let us accomplish our
goal.


### Why we chose Dask originally, and how these reasons have changed over time.

Our code consists of a mixture of legacy research-quality code and newer
production-quality code (mostly Python). Before I started we were using Google
Cloud Dataflow (Python 2 only, massively scalable but generally an astronomical
pain to work with / debug) and multiprocessing (something like ~96 cores max).

Dask let us scale beyond a single machine with only minimal changes to our data
pipeline. If we had been starting from scratch I think it's likely we would
have gone in a different direction (something like C++ or Go microservices,
especially since we have strong Google ties), but from my perspective as a
hybrid infrastructure engineer/data scientist, having all of our models in
Python makes it easy to experiment and debug statistical issues.


### Some of the pain points of using Dask for our problem

There is lots of special dask knowledge that only I possess, for example:

-   In which formats we can serialize data that will allow for it to be
    reloaded efficiently? Sometimes we can use parquet, other times it should
    be CSVs so we can easily chunk them dynamically at runtime

-   Sometimes we load data on the client and scatter to the workers, and other
    times we load chunks directly on the workers

-   The debugging process is sufficiently more complicated compared to local
    code that it's harder for other people to help resolve issues that occur
    on workers

-   The scheduler has been the source of most of our scaling issues: when
    the number of tasks/chunks of data gets too large, the scheduler tends
    to fall over silently in some way.

    Some of these failures might be to Kubernetes (if we run out of RAM, we
    don't see an OOM error; the pod just disappears and the job will restart).
    We had to do some hand-tuning of things like timeouts to make things more
    stable, and there was quite a bit of trial and error to get to a relatively
    reliable state

-   This has more to do with our deploy process but we would sometimes
    end up in situations where the scheduler and worker were running
    different dask/distributed versions and things will crash when tasks
    are submitted but not when the connection is made, which makes it
    take a while to diagnose (plus the error tends to be something
    inscrutable like `KeyError: ...` that others besides me would have no
    idea how to interpret)


### Some of the technology that we use around Dask

-   Google Kubernetes Engine: lots of worker instances (usually 16 cores each), 1
    scheduler, 1 job runner client (plus some other microservices)
-   Make + Helm
-   For debugging/monitoring I usually kubectl port-forward to 8786 and 8787
    and watch the dashboard/submit tasks manually. The dashboard is not very
    reliable over port-forward when there are lots of workers (for some reason
    the websocket connection dies repeatedly) but just reconnecting to the pod
    and refreshing always does the trick
