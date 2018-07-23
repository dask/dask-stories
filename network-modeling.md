Mobile Network Modeling
========

Who am I?
---------

I am [Sameer Lalwani](https://www.linkedin.com/in/lalwanisameer/), and I specialize in modeling wireless networks. 

We use these models to help operators with their technology decisions by building Digital Twins of their Networks. These models are used to quantify the impact of technology on user experience, network KPI & Economics.

The Problem I'm trying to solve
-------------------------------

There are always new technologies which vendors would like operators to deploy. Operators face a lot of uncertainty whenever they have to make a decisions about a new technology. These models help reduce uncertainty and allow Senior Management to gain Actionable Insights. 

For example currently we are in the middle of a 4G to 5G transition and some of the decisions that operators need to make are given below.

- Which Spectrum Band should they bid on?
- What type of User Experience and Network Performance can they expect.
- How much Capital & Operating expense will be required?
- What Site locations should they target to meet coverage and capacity requirements.
- Should they consider refarming or acquire new bands?  

These models are used to create Digital Twins at the scale of a city or a country. Based on known subscriber behavior and data from census, traffic and other sources "synthetic subscribers" are created. These subscribers could easily run into few Millions with every individual having its own physical location on a map.
We also bring in other GIS layers like Building Layouts, roads etc. to model indoor and outdoor subscribers.

The Network can also have a range of cell sites going from few Hundreds to Tens of Thousands.

Our models calculate data rates at an individual subscriber level by computing RF pathloss loss & SINR from all nearby sites. This takes our datasets into Tens of Millions rows on which numerical computation needs to be done.
  
 We also run lots of scenarios on this network to understand its capabilities and limitations.

How Dask helps
--------------
#### Dask delayed Applications
Dask Distributed helps run our scenarios across multiple machines while remaining within the memory constraint of each machine. We create approx 10-20 machines on our companies VMware infrastructure with one linux machine running dask schedular. All the other machines have identical conda environments with number of workers that match with their CPU count.

Our team runs jupyter notebooks on their desktops which connect with the dask-scheduler. Their notebooks work on their own datasets but for specialized functions and scenarios the tasks get transparently sent to the dask scheduler.

Each site count scenario takes between 10-15 mins to run, its nice to have 40 of them running in parallel.    

#### Dask Dataframe Applications
We use LiDAR data sets to calculate Line of Sight for mmWave propogation from lamposts. The LiDAR data sets for the full city are often too large to open on a single machine. Dask Dataframe allows us to pool the resources of multiple machines while keeping our logic similar to Pandas.

We use delayed to process multiple LiDAR files in parallel and then combine them into a single Dask Dataframe representing the full city. 
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

All our deployments are manually done. We bring up these machines for our analytics with identical conda environments  and tear them down once we are done.