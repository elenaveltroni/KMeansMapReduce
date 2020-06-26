## K-means with MapReduce

### K-means algorithm
The *K-means* algorithm is one of the simplest unsupervised learning algorithms used in clustering problem. It is employed to classify a given data set through a certain number of clusters fixed *a priori*.

The main idea is to define k centroids, one for each cluster, and taking each point from the given data set to associate it to the nearest centroid. When no point is pending, the next step is to re-calculate k new centroids as barycenters of the previous resulting clusters. With these k new centroids, a new binding has to be done between the same data set points and the nearest new centroid generating a loop in which the k centroids change their location step by step until no more changes are done. Finally, this algorithm aims at minimizing the squared error function of the distance measure between a data point and the cluster centre as an indicator of the distance of the n data points from their respective cluster centres.

### MapReduce programming model
Implementing a scalable version of \textit{k-means} is useful in case of large datasets. In order to do this, the MapReduce programming model will be used, parallelizing the problem using a cluster of 4 machines, and it will be implemented using the Hadoop and Spark framework.

More in detail, the whole MapReduce process goes through four steps of execution:
- **Splitting**: The input to the MapReduce job is divided into fixed-size pieces that will be consumed by a single map;
- **Mapping**: Data in each split is passed to a mapping function to produce as output key-value pairs;
- **Shuffling**: Nodes redistribute data based on the output keys produced by the map function, such that all data belonging to one key is located on the same worker node;
- **Reducing**: Values from Shuffling phase are combined processing each group of output data, per key, in parallel, and a single output value is returned. If more Reducers are used, the MapReduce system collects all the Reduce output and sorts it by key to produce the final outcome.

### Hadoop framework
In this project, the following structure of MapReduce algorithm will be used:

The Mapper algorithm takes as input a point and, after reading the centroids, initially randomly created, from a file, assigns it to te nearest centroid. At the end emits a pair of centroid-point.
![Map method](https://github.com/davide-coccomini/kmeans-mapreduce/)

The Combiner algorithm takes as input a centroid and a list of points and first computes the list dimension, then for each point in the list calculates the partial sum. At the end emits a pair of centroid and partial sum.
![Combine method](https://github.com/davide-coccomini/kmeans-mapreduce/)

The Reducer algorithm takes as input a centroid and a list of partial sums. As the Combiner, it computes the list dimension and for each partial sum belonging to the list calculates the new centroid as the ratio between the total sum and the list dimension. At the end emits the centroid index and the new centroid.
![Reduce method](https://github.com/davide-coccomini/kmeans-mapreduce/)
