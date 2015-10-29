# Big Data Engineering: The Hitchhiker's Guide

* Database: A system allowing for storing data and getting back at a later point
  * Flat-file: No concurrent access
  * Relational: Avoids data redundancy, has concurrent access. Problems with scaling
  * OLAP systems: Rearranged data to improve performance for subset of relational queries.

  ## CAP Theorem

  * Eric Brewer came up with it
  * Distributed systems have 3 criteria: availability, consistency and partition tolerance
    * Partition tolerance: If it doesn't have it, cannot be spread over a cluster of nodes
    * Relational databases have consistency and availability, but no partition tolerance
  * Can only really guarantee 2 of these things
  * Brewer also said that these properties are "more continuous than binary," so there is a spectrum.

## NoSQL

* Optimize your data by targeting specific scenarios. There is no real normalization.
* NoSQL DBs are typically eventually consistent
* Key-Value stores, Document Stores, Columnar Stores, Graph Stores

## Big Data

* Large data sets, take minutes to query and return results


## Hadoop

* Project sparked by Google's MapReduce paper
* Uses HDFS for storage
  * Reliable data storage model on cheap and unreliabale commodity hardware
  * Optimized for high throughput and large data
* Data processing model is MapReduce
  * Takes processing to data, instead of bringing data to processing
  * Queries are split and distributed across nodes
  * Data is combined in Reduce step
* Implementation is implementing two interfaces in Java: Map and Reduce
* Hive: SQL-like language for managing queries
* ETL with Pig, query with Hive.

### Issues

* Workflow and scheduling can be tough
* Shellscripts and cron?
* Apache Oozie manages workflow
* Another problem is that batch processing is high latency
  * Need real-time solutions for real-time Problems
* Solution: Apache Storm

## Apache Storm

* Project came from Twitter
* Written in CLojure and Java
* Real-time computation. Ideal for stream processing and continuous computation
* Open ended stream of data is channeled through bolts, output stream on the other side
* Java API, Clojure API, CLI
* Want decoupling between producers and consumers. Can use Kafka

## Kafka

* Pub/Sub system, where consumers must pull data from the topics
* Java API and CLI

## Lambda Architecture

* Coined by Nathan Maraz
* Decoupling batch processing and real-time processing
* Combines "speed layer" that processes recent data, and for rest of data you use a "batch layer" which updates more slowly.
* Challenges: Need to still write code, compile, build, deploy, run, wait, output, start again

## Apache Spark

* Adds interactivity into data
* Cluster computing with working sets
* Data anlytics cluster computing
* In-memory cluster computing. Allows aps to load data into the memory of a cluster and query it repeatedly
* Built atop of HDFS but not tied to Hadoop
* Platform on top of Spark
  * Shark (Hive Support)
  * Spark Streaming
  * GraphX
  * MLib (Machine Learning Library out of the box)
* Can install Spark as a YARN application (Hadoop 2.0)
* Upcoming: Java 8 support, Spark SQL, BlinkDB for approximations (approximate age of every person on earth), Spark R
* Much faster than Storm
* RDD (Resilient Distributed Datasets)
  * Immutable, In-memory and Fault-tolerant
  * Can choose cached in memory, just disk, or both
* Interface: spark-shell (Scala), pyspark (Python)

## Enterprise Data Lake

* All data is stored in Hadoop
* All data is raw
  * Schema on read as opposed to schema on write
  * No data modeling
* All data is accessible
* All other data processing systems are downstream including the data warehouse
