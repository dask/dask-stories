# Discovering biomarkers for rare diseases in blood samples

## Who am I?
I am Markus Schmitt and I am CEO at [Data Revenue](https://www.datarevenue.com/). We build custom machine learning solutions in a number of fields, including everything from medicine to car manufacturing.

## What problem am I trying to solve?
It's hard for doctors to diagnose rare diseases based on blood tests. Usually patients are subjected to expensive and prolonged semi-manual gene testing.

We are analysing thousands of blood samples, comparing sick and healthy patients. We look for biomarkers, compounds in the blood, such as iron, and these help doctors to identify people who have rare diseases (and those who do not).
Currently, this is done offline, on historical samples, but with more work this could be used in real-time too: to analyse patients' blood and give them feedback faster and more cheaply than is currently possible.

## How does Dask help?
We started the project without Dask, writing our own custom multiprocessing functionality. This was a burden to maintain, and Dask made it simple to switch over to thinking at a distributed acyclic graph (DAG) level. It was great to stop thinking about individual cores.

Dask has allowed us to run all of our analysis in parallel, shortening the overall feedback loop and letting us get results faster.
We've found Dask to be extremely flexible. We have used it extensively to help with our distributed analysis, but Dask adds value for us in simpler cases too. We have systems which revolve around user-submitted jobs, and we can use Dask to help schedule these, whether or not 'big data' is involved.

## Why did I choose Dask?
After it became clear that we were wasting significant time maintaining our custom multiprocessing code, we considered several alternatives before choosing Dask. We specifically considered
-   [Apache Flink](https://flink.apache.org/): we found this not only lacking some functionality that we needed, but also very complex to set up and maintain.
-   [Apache Spark](https://spark.apache.org/): we similarly found this to be a time sink in terms of set up and maintenance.
-   [Apache Hadoop](https://hadoop.apache.org/): we found the MapReduce framework too restrictive, and it felt like we were always pushing round pegs into square holes.
Ultimately, we chose Dask because it offered a great balance of simplicity and power. It is flexible enough to let us do nearly everything we need, but simple enough to not put a maintenance burden on our team.
We use the Dask scheduler along with the higher level APIs. Our data fits well into a table structure, so the DataFrame API provides a lot of value for us.

## Pain points
Dask has largely done exactly what we want, but we have had some memory issues. Sometimes, because of the higher layer of abstraction, it's not easy to predict exactly what data will be loaded into memory.
In some cases, Dask loads far more data into memory than we expected, and we have to add custom logic to delay the processing of some tasks. These tasks are then only run when previous tasks have completed and memory is freed up.
Because we work with medical data, we also have a strong focus on security and compliance. We found the Dask integration with Kubernetes to be lacking some features we needed around this. Specifically we had to apply some manual SSL patches to ensure that data was always transferred over SSL.
But overall, wherever Dask has fallen short of our needs, it has been easy for us to patch these. Dask's architecture makes it easy to understand and change as needed.

## Technology I use around Dask
We run Dask on Kubernetes on AWS, with batch processing managed by [Luigi](https://luigi.readthedocs.io/en/stable/). We use scikit-learn for machine learning, and [Dash](https://www.datarevenue.com/ml-tools/dash) for interactive GUIs and Dashboards.

## Anything else to know?
We use Dask in many of our projects, from smaller ones all the way up to our largest integrations (for example with Daimler) and we have been impressed with how versatile it is. We process several billion records, amounting to many terabytes of data, through Dask every day.

## Links
- [Scaling Pandas: Comparing Dask, Ray, Modin, Vaex, and RAPIDS](https://www.datarevenue.com/en-blog/pandas-vs-dask-vs-vaex-vs-modin-vs-rapids-vs-ray)
- [How to Scale your Machine Learning Pipeline with Dask](https://www.datarevenue.com/en-blog/how-to-scale-your-machine-learning-pipeline)
