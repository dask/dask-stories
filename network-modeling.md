Mobile Network Modeling
=======================

Who am I?
---------

I am [Sameer Lalwani](https://www.linkedin.com/in/lalwanisameer/), and I specialize in modeling wireless networks.

We use these models to help operators with their technology decisions by building digital twins of their wireless networks. These models are used to quantify the impact of technology on user experience, network KPI & economics.


The problem I'm trying to solve
-------------------------------

Mobile network operators face uncertainty whenever a new technology emerges.  Their existing networks are complex with several bands and pre-existing sites.  They need to understand the implications of bringing a new technology into this mix.  By modeling their networks we can help reduce the uncertainty that management faces, while giving them actionable insights about their network.

For example currently we are in the middle of a 4G to 5G transition, presenting operators with decisions like the following:

- Which spectrum band should they bid on?
- What type of user experience and network performance can they expect?
- How much capital & operating expense will be required?
- What site locations should they target to meet coverage and capacity requirements?
- Should they consider refarming or acquire new bands?

These models are used to create digital twins at the scale of a city or a country. Based on known subscriber behavior and data from census, traffic and other sources "synthetic subscribers" are created. These subscribers could easily run into few millions with every individual having its own physical location on a map.
We also bring in other GIS layers like building layouts, roads etc. to model indoor and outdoor subscribers.

The network can also have a range of cell sites going from few hundreds to tens of thousands.

Our models calculate data rates at an individual subscriber level by computing RF pathloss loss & SINR from all nearby sites. This takes our datasets into tens of millions of rows on which numerical computation needs to be done.

We also run lots of scenarios on this network to understand its capabilities and limitations.


How Dask helps
--------------

### Distributed computing and Dask delayed

Dask Distributed helps run our scenarios across multiple machines while remaining within the memory constraints of each machine. We create approximately 10-20 machines on our VMware infrastructure with one Linux machine running the Dask scheduler, and all other machines running Dask workers with identical Conda environments.

Our team runs Jupyter notebooks on their desktop machines which connect with the Dask scheduler. Their notebooks work on their own datasets but for specialized functions and scenarios their tasks get transparently sent to the Dask scheduler.  Each site count scenario takes between 10-15 mins to run, so it's nice to have 40 of them running in parallel.

### Dask Dataframe

We use LiDAR data sets to calculate line of sight for mmWave propagation from lamp posts. The LiDAR data sets for the full city are often too large to open on a single machine. Dask Dataframe allows us to pool the resources of multiple machines while keeping our logic similar to Pandas dataframes.

We use Dask delayed to process multiple LiDAR files in parallel and then combine them into a single Dask Dataframe representing the full city.  The line of sight calculation is CPU intensive and so it is nice to distribute it across multiple cores.

One of the things we really appreciate about Dask is that it gives us the ability to focus on the model logic without worrying about scalability. Once the code is ready, its seamless to call the functions using delayed and have them run across multiple CPU cores.


Pain points when using dask
---------------------------

I find Dask Dataframe to be too slow for multi column groupby operations. For such tasks I sort the Dataframe by the columns and partition it by the first column. I then use a delayed operation to use pandas on each partition which end up being much faster.


Technology I use around Dask
----------------------------

- GIS : Geopandas, Would love to get this natively supported.
- Traffic Flows: networkx
- Analytics: scikit-learn, scipy, pandas
- Visualization: Datashader with Dask distributed for LiDAR data.
- Charts: Holoviews, Seaborn
- Data Storage: HDF5

All our deployments are manually done. We bring up these machines for our analytics with identical Conda environments and tear them down once we are done.

Previously we used IPython parallel but found that Dask was easier to set up, and also allowed us to write more complex logic and also share our computing pool between multiple users.


Links to this work
------------------

Examples of models which use LiDAR to calculate line of sight:

1. [5G mmWave Coverage from Lamp posts](https://www.linkedin.com/pulse/how-much-5g-coverage-you-really-get-from-lampposts-sameer/)
2. [mmWave Backhaul & Economics](https://www.linkedin.com/pulse/why-terragraph-mmwave-backhaul-essential-5g-lalwani-mba-ms-ee/)
