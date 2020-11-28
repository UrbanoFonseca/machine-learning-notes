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

## RDDs

RDDs are Resilient Distributed Datasets, a fault-tolerant collection of elements that can be operated in parallel. To compute a transformation against each partition, you will require a thread assigned to a task.

RDDs that are shared for multiple purposes inside the same code block are good candidates for being chached, i.e. their partitions are persisted in disk and/or memory on the executors such that their subsequent usage is much faster. Actually, **caching** is a lazy evaluation: when you are scripting and adding transformations to your DAG Spark will not persist the data right away, it will wait untill you command an action and it will cache the data only when the execution passes through that command. For performance purposes, you should cache on the most downstream step possible, such that all the ETL transformations calculated up until the point where you share the RDD \(i.e. execute different operations for different purposes\) are not duplicated.

## Ways to Run Spark

**Local** mode is basically running Spark in one JVM on one machine containing both the Executors and the Driver, with static partitioning. Spark denotes the slots \(able to run tasks\) as cores. You should oversubscribe by a factor of 2x/3x the number of logical cores of the machine in which you are running. The threads are not pinned directly to the cores, there is an abstraction layer of the JVM and OS kernel inbetween the thread slots and the actual cores. At the same time, Spark is running internal threads \(e.g. for shuffle purposes\) which are mostly sitting idle. You can start local mode with the `--master` option to `local`\(one core\), `local[N]` using N cores or `local[*]` which uses the number of available logical cores \(which as previously discussed does not make much sense\).

**Standalone Scheduler** is able to run on a cluster environment but it has a static partitioning. When starting the machines, certain JVMs will instantiate such as the Spark Master JVM. This is not the same as instantiating a Spark App. On each machine, a Worker JVM will startup and they will register with the Spark Master. Those JVMs are not the Spark Application, they can fare with 1GB of RAM for either the workers or the spark master. When we submit our application using spark-submit, a Driver will start wherever we specified and it will contact the Spark Master telling that it will require Executor JVMs to run the App. The Spark Master is a scheduler that decides where to launch the executor jvms, ordering the workers to start the specified executor jvms \(default of 1 per each machine\). All the Spark Master is doing is deciding and scheduling where each executor should run. The Worker is just responsible for starting up the required JVMs, but if they crash will restart the executor JVM. If the worker crashes, the Spark Master will restart it. 

 This notion of **static partitioning** is not the same as RDDs, what it means is rhat when you launch your Spark application you will be stuck with the configurations you request. On the other side, with **dynamic partitioning** you can adapt the number of executors in the mid-life of the Spark App.

If the partitions needed to execute a task are persisted within the same machine, the partitions go directly from the executor's JVM heap right to the thread.

## spark-env.sh

Within the `spark-env.sh` you can define the environment variables, such as `SPARK_LOCAL_DIRS` which are used to store RDDs when they are persisted on disk and to store intermediate shuffle data. When you persist data that does not fit in memory alone, spark still reads the data from the external source \(e.g. cassandra, hdfs\), brings into the executor JVM and then spills those partitions down into the local directories. This means that recomputing against those partitions can be made directly by loading them from disk to memory instead of reading the data from the original data source. Plus, if you use multiple storages \(e.g. SSDs attached to the same machine\) Spark will push the data in parallel into those multiple drives.

The `SPARK_WORKER_CORES` allow you to specify the number of cores that worker will allow to give to its underlying executors.

| Parameter | Description | Default |
| :--- | :--- | :--- |
| `SPARK_WORKER_INSTANCES` |  \# workers instances running each machine | 1 |
| `SPARK_WORKER_CORES` | \# cores allowed for Spark Apps on that machine | ALL |
| `SPARK_WORKER_MEMORY` | Total memory allowed for Spark Apps on that machine | TOTALRAM-1GB |
| `SPARK_DAEMON_MEMORY` | Memory to allocate Spark Master and Workers | 512 MB |

## Hadoop Recap

On HDFS, a **Name Node** heartbeats regularly with the **Data Nodes** JVMs. Spark does not replace HDFS. Hadoop is 3 projects: HDFS, YARN and MapReduce. The latter can be replaced by Spark core.

For MapReduce, a **JobTracker** is the brain and the **TaskTracker** JVMs are running on each machine. Under the TaskTracker, you will have either Map or Reduce child JVMs.

> With the traditional MapReduce, you get parallelism by adding process ids and multiple JVMs for the Map and Reduce processes. In Spark, you can get parallelism with different threads running inside the executor.

The slots in MapReduce are hard-coded to be either Map or Reduce slots, which means that everything is launched at start and is kept idle but taking resources from the cluster. A Reduce task in the end will keep those resources from being used by the earlier Map stages. In Spark, the Slots inside the Executors are generic.

In Spark, you will have an Executor JVM in each machine, inside that JVM you will have Slots, in those Slots you can run Tasks.

