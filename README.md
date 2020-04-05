# ML ALGORITHMS IN DISTRIBUTED SYSTEMS

This repo contains the implementation of common algorithm used for machine learning such as SGD, KNN in distributed system. 

## 1. Purpose:
Practice the design of distributed system. In this case, my implementation is on Spark. I would like to investigate the disadvantages, advantages of distributed system in each cases. It is not wise to always apply the distributed version of algorithm for all cases. 

## 2. Approach:
All the notebooks in this repo follow a similar approach:

1. Implement a normal version of algorithm
2. Verify the correctness of this implementation
3. Improved version of this algorithm if exist.
4. Implement the distributed version of algorithm in Spark.
5. Analysis of this version, compare with the normal version.


Since these notebook is **quite long**, I will provides the summary of what I found in the following sections. 

## 3. Results
### 3.1. Stochastic Gradient Descent (SGD)
From the machine learning perspective, my implementation confirmed the well-known fact that Mini-Batch-SGD is in the middle of Batch Gradient Descent and Mini-Batch Stochastic Gradient Descent, in terms of convergence-rate and fluctuation. It's easier to tune, using learning-rate, than SGD.

From the distributed system perspective, by investigate the implementation of distributed version, I had a couple of observations:
-  choosing optimized value for number of partition given the cluster configuration (using information from Spark Master Web UI) to maxmize the data locality, minimize data transfer
-  *reduce()* is the bottle neck of system in this implementation and **treeReduce/treeAggregate** is these alternatives to reduce the load the driver has to deal with, so improve the system's performance.
-  When dataset's size is small, it is true that the distributed algoritms will take much more time in comparison to serial algorithms. Because in small datasets, the time-cost for transfering data between machines, waiting for synchronous updates, etc. outweight the time-cost of calculating gradients, updating thetas, etc.

### 3.2. 