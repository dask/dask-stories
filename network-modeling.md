Mobile Network Modeling
========

Who am I?
---------

I am [Sameer Lalwani](https://www.linkedin.com/in/lalwanisameer/), and I specialize in modeling wireless networks. 

We use these models to help operators with their technology decisions by building Digital Twins of their Networks. These models are used to quantify the impact of technology decisions on network KPI & Economics.

The Problem I'm trying to solve
-------------------------------

There are always new technologies which vendors would like operators to deploy. There are a lot of decision operators need to make before they can make a decision on deploying new technology.

For example currently we are in the middle of a 4G to 5G transition and some of the decisions that operators need to make are given below.

- Which Spectrum Band should they bid on?
- What type of User Experience and Network Performance can they expect.
- How much Capital & Operating expense will be required?
- What Site locations should they target to meet coverage and capacity requirements.
- Should they consider refarming or acquire new bands?  


How Dask helps
--------------

These models create a Digital Twin at the scale of a city or a country in which every subscriber is simulated. Based on known subscriber behavior and data from census, traffic and other sources "synthetic subscribers" are created. These subscribers could easily run into few Millions with every individual having its own physical location on a map.
We also bring in Building Layouts to model indoor and outdoor subscribers.

The city can also have a range of cell sites going from few hundreds to Tens of Thousands.

Our models calculate data rates at an individual subscriber level with the ability of running lots of scenarios.

Dask Distributed helps run our scenarios across multiple machines while remaining within the memory constraint of each machine. We use Dask Delayed for this operation.

We also use LiDAR data sets to calculate Line of Sight for mmWave propogation from lamposts. The LiDAR data sets for the full city are often too large to open on a single machine and Dask Dataframe allows us to pool the resources of multiple machines while keeping our logic similar to Pandas.
The Line of Sight calculation is CPU intensive and is distributed across multiple cores.

Examples of models which use LiDAR to calculate Line of Sight.
1. [5G mmWave Coverage from Lamposts](https://www.linkedin.com/pulse/how-much-5g-coverage-you-really-get-from-lampposts-sameer/) 
2. [mmWave Backhaul & Economics](https://www.linkedin.com/pulse/why-terragraph-mmwave-backhaul-essential-5g-lalwani-mba-ms-ee/)


I used to use iPython parallel for some of my work but it was a pain to setup and debug. Dask delayed solved all those problem plus we could write much more complex logic and share our computing pool between multiple users.

One of  the thing I really appreciate about dask is that it gives me the ability to focus on the model logic without worrying about scalability. Once the code is ready, its seamless to call the functions using delayed and have them run across multiple CPU cores.

Pain points when using dask
---------------------------

I find Dask Dataframe to be too slow for multi column groupby operations. For such tasks I sort the Dataframe by the columns and partition it by the first column. I then use a delayed operation to use pandas on each partition which end up being much faster.

Technology I use around Dask
----------------------------

- GIS : geopandas, Would love to get this natively supported.
- Traffic Flows: networkx
- Analytics: scikit-learn, scipy, pandas
- Visualization: Datashader with Dask distributed for LiDAR data. 
- Charts: Holoviews, Seaborn 
- Data Storage: HDF5
