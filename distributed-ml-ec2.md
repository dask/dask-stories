# Distributed machine learning on Amazon EC2

## Who am I?

I am [Scott Sievert], a graduate student at the University of Wisconsin–Madison
studying distributed machine learning. I study how to train deep neural nets
more quickly.

## What problem am I trying to solve?
Communication and computation of results are involved in distributed or
multi-machine calculation. In distributed machine learning
they're both criticial to performance (wall-clock time) because the datasets or
models are large and the computation is involved. Typically, communication
is the speed bottleneck.

We want to make distributed machine learning training faster by minimizing the
amount of communication required. We developed some result backed by theory,
but this result depended
on one "hyperparameter" or "user-chosen variable" that would critically
effect performance.

We wanted to "tune" or choose the best value for this hyperparameter.
Tuning required an adaptive search to minimize the compute time:
every evaluatation would depend on all previous evaluations to use
as much information as possible in hopes of minimizing computation time.
This created a problem because we wanted to tune this hyperparameter for
every combination of

* 4 networks (2, 4, 8 and 16 machines),
* 6 different algorithms for comparison, and
* 2 different models

Our problem formulation is best suited for one GPU per compute node, so 
we had to use a p2.xlarge Amazon EC2 instance. It is the only instance
to have exactly one GPU large and fast enough for our models, and be somewhat available
on the spot instance market.

Any one combination of network, algorithm and model took 1 hour to complete,
so in serial this tuning would take 48 hours to complete. However, Amazon
has constraints for our EC2 instance of choice: any p2.xlarge cluster is
automatically shutdown after 6 hours and the cluster size is
practically limited to 16 machines.

I eventually got around these constraints with Dask, though the results
aren't used in our paper,
"[ATOMO: Communication-efficient Learning via Atomic Sparsification][paper]"
because we wanted quicker iteration time.

## How Dask helped me
Dask can easily manage clusters of various machines, which means I can
create a cluster to manage my hyperparameter optimization. However, I
wanted to

* use a custom high-performant PyTorch cluster management solution for any one experiment
* evaluate experiments via an adaptive hyperparameter search

The biggest way Dasks helped this process is with [launching tasks
from tasks][tasks], which is useful in this context because I wanted
to have nested parallelism: for all 6 models and a given network structure,
launch some parallel adaptive search. This even inspired [a blog post] by me
on this.

[tasks]:https://distributed.readthedocs.io/en/latest/task-launch.html

All of this was straighforward with Dask.distributed, after
discovering the right techniques and methods.

## Pain points of using Dask

* Trying to manage sets of PyTorch clusters with Dask.
  I needed this to be dynamic because of the 6 hours time
  limits.
  
I don't think this is a pain point of using Dask. I think
the fact that it is a pain point is a feature of  Dask. This
problem is native to my EC2 setup, and Dask allowed it to
become a problem.

## Technology that we use around Dask
[PyTorch], my favorite deep learning library.

[paper]:https://arxiv.org/abs/1806.04090
[a blog post]:https://stsievert.com/blog/2018/01/04/dask-get-client/
[Scott Sievert]:https://stsievert.com/
[PyTorch]:https://pytorch.org/
