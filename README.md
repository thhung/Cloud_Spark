# ML ALGORITHMS IN DISTRIBUTED SYSTEMS

This repo contains a short series of projects to demonstrate different use-cases in distrubted system. In this series, there are the implementations of common algorithms such as SGD, KNN in distributed system and a project of data analysis with Spark. 

## 1. Purpose:
- Practice the design of distributed system.
- Investigate the disadvantages, advantages of distributed system in each use-cases(it is not wise to always apply the distributed version of algorithm for all cases). 

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
From the algorithm's perspective, my implementation confirmed the well-known fact that Mini-Batch-SGD is in the middle of Batch Gradient Descent and Mini-Batch Stochastic Gradient Descent, in terms of convergence-rate and fluctuation. It's easier to tune, using learning-rate, than SGD.

From the distributed system perspective, by investigate the implementation of distributed version, I had a couple of observations:
-  choosing optimized value for number of partition given the cluster configuration (using information from Spark Master Web UI) to maxmize the data locality, minimize data transfer
-  *reduce()* is the bottle neck of system in this implementation and **treeReduce/treeAggregate** is these alternatives to reduce the load the driver has to deal with, so improve the system's performance.
-  When dataset's size is small, it is true that the distributed algoritms will take much more time in comparison to serial algorithms. Because in small datasets, the time-cost for transfering data between machines, waiting for synchronous updates, etc. outweight the time-cost of calculating gradients, updating thetas, etc.

### 3.2. K-mean
* Machine Learning's perspective:
	- KMean is:
		+ fast and efficient, can be strictly converged
		+ good at capturing spherical-like structure of the dataset, depends heavily on the initial centroids, sensitive to outliers

* Distributed System's perspective:
	Caching mechanism of Spark:
	- Caching is one mechanism to speed up applications that acess the same RDD multiple times.
	- There are two functions for caching an RDD: cache() and persist(StorageLevel):
		+ cache() will cache the RDD into memory
		+ persist(level) can cache in memory, on disk, or off-heap memory according to the caching strategy specified by StorageLevel


### 3.3 SPARKSQL
In this project, I did only use the data analysis to demonstrate the utilisation of SparkSQL to find the insights about a flight dataset. 
* Distributed System's perspective:
	- DataFrame organizes underhood RDDs into a structured table, allowing SparkSQL to optimize the query. Therefore, DataFrame API is faster than RDD.



