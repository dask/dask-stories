Dask Use Cases
==============

Dask is a versatile tool that supports a variety of workloads.
This page contains brief and illustrative examples of how people use Dask in practice.
These emphasize breadth and hopefully inspire readers to find new ways
that Dask can serve them beyond their original intent.

.. toctree::
   :maxdepth: 1

   sidewalk-labs.md
   mosquito-sequencing.md
   fullspectrum.md
   icecube-cosmic-rays.md
   pangeo.md
   hydrologic-modeling.md
   network-modeling.md
   satellite-imagery.md
   prefect-workflows.md

Overview
--------

Dask uses can be roughly divided in the following two categories:

1.  Large NumPy/Pandas/Lists with
    `Dask Array <https://docs.dask.org/en/latest/array.html>`_,
    `Dask DataFrame <https://docs.dask.org/en/latest/dataframe.html>`_,
    `Dask Bag <https://docs.dask.org/en/latest/bag.html>`_,
    to analyze large datasets with familiar techniques.
    This is similar to Databases, Spark_,  or big array libraries

2.  Custom task scheduling.  You submit a graph of functions that depend on
    each other for custom workloads.  This is similar to Luigi_, Airflow_,
    Celery_, or Makefiles_

Most people today approach Dask assuming it is a framework like Spark, designed
for the first use case around large collections of uniformly shaped data.
However, many of the more productive and novel use cases fall into the second
category where Dask is used to parallelize custom workflows.

In the real-world applications above we see that people end up using both
sides of Dask to achieve novel results.

Contributing
------------

If you solve interesting problems with Dask then we want you to share your
story.  Hearing from experienced users like yourself can help newcomers quickly
identify the parts of Dask and the surrounding ecosystem that are likely to be
valuable to them.

Stories are collected as pull requests to `github.com/dask/dask-stories
<https://github.com/dask/dask-stories>`_.  You may wish to read a few of the
stories above to get a sense for the typical level of information.  There is a
template in the repository with suggestions, but you can also structure your
story a different way.

.. toctree::
   :maxdepth: 1

   template.md

.. _Airflow: https://airflow.apache.org/
.. _Luigi: https://luigi.readthedocs.io/en/latest/
.. _Celery: http://www.celeryproject.org/
.. _Spark: https://spark.apache.org/
.. _Makefiles: https://en.wikipedia.org/wiki/Make_(software)
