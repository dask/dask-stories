### Who am I?

My name is [Hussain Sultan](https://www.linkedin.com/in/hussainsultan/).
I am a partner at [Full Spectrum
Analytics](https://www.fullspectrumanalytics.com/).  I create personalized
analytics software within banks for the sake of equitable and profitable
decision making.


### What problem am I trying to solve?

Lending businesses create and manage valuations and cashflow models that output
the profitability expectations for customer segments. These models are complex
because they form a network of equations that need to be scored efficiently and
keep track of inputs/outputs at scale.


### How Dask helps

Dask is instrumental in my work for creating efficient cashflow model
management systems and general data science enablement on data lakes.

Dask provides a way to construct the dependencies of cashflow equations as a
DAG (using the [dask.delayed](https://dask.pydata.org/en/latest/delayed.html)
interface) and provides a good developer experience for building
scoring/gamification/model tracking applications.


### Why I chose Dask originally

I chose dask for three reasons:

1.  It was lightweight
2.  The granular task scheduling approach to scaling both dataframes and
    arbitrary computations fit my use case well
3.  It is easy to scale my team with Python programmers


### Some of the pain points of using Dask in our problem

It's hard to get organization buy-in to adopt an open-source technology without
vendored support and enterprise SLAs.

In a recent project, we had to integrate with the Orc data format that turned
out to be more expensive than I originally anticipated (compounded by
enterprise hadoop set-up and encryption requirements).  These changes have
since been upstreamed though, and so things are easier now.


### Some of the technology that we use around Dask

We deployed on generic internal server with Jenkins scheduling a Jupyter
notebook to execute.  We built everything out using our internal analytics
platform.  We didn't have to worry about security because everything was behind
a corporate firewall.
