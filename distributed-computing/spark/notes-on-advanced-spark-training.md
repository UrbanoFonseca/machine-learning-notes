---
description: Advanced Apache Spark Training - Sameer Farooqui (Databricks)
---

# Notes on Advanced Spark Training

## iEcosystem

> Spark is a scheduling, monitoring and distribution engine for big data.

At the center, **Spark Core** uses memory and disk while it is processing data. There are 5 APIs: Python, R, Java, Scala and DataFrames. Around this, we have **high-level libraries** \(e.g. SQL, Streaming, MLLib, GraphX\) which push the compute down to Spark Core. Above, we have **resource managers** which will coordinate the running and execution of spark: Local, YARN, Mesos \(original RM\) and Standalone. The distributed RMs \(last 3\) can be made available with Zookeper.

Outside the perimeter, we have filesystems \(e.g. HDFS, local\), databases \(e.g. mongoDB, Cassandra\). Also, Flume and Kafka can be buffers for Spark Streaming.

The main goal is to be an General Unified Engine that is able to run multiple Specialized Systems, extending the original MapReduce Hadoop framework.

## MapReduce

Previously, using MapReduce for multiple stage calculations would require writing the outputs of each MapReduce stages into HDFS and load it again from the filesystem, which would lead to complex coordination and a bottleneck on I/O alone. Spark allows to keep the data in memory inbetween the multiple jobs and write the results in the end to the desired database.

| CPU Access | Read Speed /s |
| :--- | :--- |
| Memory | 10 GB |
| Hard Disk Drive | 100 MB |
| Solid State Drive | 600 MB |
| Network | 125 MB \(1Gb\) |

For reference, reading speed is 1Gb/s \(125MB/s\) from nodes within the same rack and 0.1Gb/s for nodes in other racks.

