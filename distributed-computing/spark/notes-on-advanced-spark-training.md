---
description: Notes on Advanced Apache Spark Training - Sameer Farooqui (Databricks)
---

# Advanced Spark Training

## Ecosystem

> Spark is a scheduling, monitoring and distribution engine for big data.

At the center, **Spark Core** uses memory and disk while it is processing data. There are 5 APIs: Python, R, Java, Scala and DataFrames. Around this, we have **high-level libraries** \(e.g. SQL, Streaming, MLLib, GraphX\) which push the compute down to Spark Core. Above, we have **resource managers** which will coordinate the running and execution of spark: Local, YARN, Mesos \(original RM\) and Standalone. The distributed RMs \(last 3\) can be made available with Zookeper.

Outside the perimeter, we have filesystems \(e.g. HDFS, local\), databases \(e.g. mongoDB, Cassandra\). Also, Flume and Kafka can be buffers for Spark Streaming.

The main goal is for Spark to be a General Unified Engine that is able to run multiple Specialized Systems, extending the original MapReduce Hadoop framework.

## Legacy

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

RDDs that are shared for multiple purposes inside the same code block are good candidates for being chached, i.e. their partitions are persisted in disk and/or memory on the executors such that their subsequent usage is much faster. Actually, **caching** is a lazy evaluation: when you are scripting and adding transformations to your DAG Spark will not persist the data right away, it will wait until you command an action and it will cache the data only when the execution passes through that command. For performance purposes, you should cache on the most downstream step possible, such that all the ETL transformations calculated up until the point where you share the RDD \(i.e. execute different operations for different purposes\) are not duplicated.

## Ways to Run Spark

The concepts of Driver and Executors exist on the multiple methods to run Spark, the key difference between them is who starts the executor.

### Overview

Spark provides an abstraction with **spark submit** that lets you develop your application code regardless of the way you execute by changing the `--master` flag and corresponding . This enables for an uniform interface for the multiple cluster managers.

> The **Spark Central Master** decides where the executors will run.

| **Mode** | Spark Central Master | Executor Spawn |
| :--- | :--- | :--- |
| **Local** | _na_ | Data Scientist |
| **Standalone** | Standalone Master | Worker JVM |
| **YARN** | YARN App Master | Node Manager |
| **Mesos** | Mesos Master | Mesos Slave |

### **Local**

**Local** mode is basically running Spark in one JVM on one machine containing both the Executors and the Driver, with static partitioning. Spark denotes the slots \(able to run tasks\) as cores. You should oversubscribe by a factor of 2x/3x the number of logical cores of the machine in which you are running. The threads are not pinned directly to the cores, there is an abstraction layer of the JVM and OS kernel in-between the thread slots and the actual cores. At the same time, Spark is running internal threads \(e.g. for shuffle purposes\) which are mostly sitting idle. You can start local mode with the `--master` option to `local`\(one core\), `local[N]` using N cores or `local[*]` which uses the number of available logical cores \(which as previously discussed does not make much sense\).

### **Standalone**

**Standalone Scheduler** is able to run on a cluster environment but it has a static partitioning. When starting the machines, certain JVMs will instantiate such as the Spark Master JVM. This is not the same as instantiating a Spark App. On each machine, a Worker JVM will startup and they will register with the Spark Master. Those JVMs are not the Spark Application, they can fare with 1GB of RAM for either the workers or the spark master. When we submit our application using spark-submit, a Driver will start wherever we specified and it will contact the Spark Master telling that it will require Executor JVMs to run the App.

The Spark Master is a scheduler that decides where to launch the executor jvms, ordering the workers to start the specified executor jvms \(default of 1 per each machine\). All the Spark Master is doing is deciding and scheduling where each executor should run. The Worker is just responsible for starting up the required JVMs, but if they crash will restart the executor JVM. If the worker crashes, the Spark Master will restart it. 

The settings are that 1. Apps are submited in FIFO by default; 2. you can request the maximum number of cores for the application from across the cluster with `spark.cores.max` and; 3. you can assign the memory per executor with `spark.executor.memory`.

### YARN

![](../../.gitbook/assets/image%20%2848%29.png)

In Yarn, a master machine runs a **ResourceManager \(RM\)** and multiple slave machines run a **NodeManager \(NM\)** each, heartbeating and sharing info with the RM \(e.g. how many cores are occupied, how much network bandwidth is being used\).

When you submit a Spark Application, the RM will find a container on some node to start the **ApplicationMaster \(AM\).** The AM will not do any work, it works similar to the JobTracker in traditional MapReduce as it will be the brain of the operation. After it is spawn, the AM contacts the RM again to request **Containers** to do the heavy work. The RM provides keys and tokens so that the AM can contact the NMs directly and start those containers. The containers will then register back with the AM. When the App is finally running, the AM provides insights directly to the client which spawned the Application and the RM is out of the loop.

Note the difference between containers and executors: when Spark runs on Yarn mode the executors will exist inside the Containers but such concept of containers could also be applicable Hadoop MapReduce jobs.

The RM contains a Scheduler and an AppsMaster. The **Scheduler** decides where the AMs will run and where the containers will be scheduled.The **ApplicationsMaster \(AppsM\)** is responsbile for tracking the health state of individual AMs. The RM is made highly available with Zookeper.

The concept of AM is not the Spark Master, it is a YARN Application Master that exists so that Containers can run Executors. 

> There are 2 ways of running Spark on Yarn, the **client** mode and the **cluster** mode.

#### Client Mode

In the Client mode, the Driver runs on the client itself, i.e. the place where you started the application. If you start a Spark shell within a SSH session, the driver will be hosted on that machine. The AM and the multiple containers still exist in the cluster nodes, but when you trigger a collect action the result RDD will be made available directly in the Driver JVM and converted into the collection suitable for how you are running the application \(e.g. Array in the Spark shell\).

The executors within the containers are in direct communication with the driver.

One key downside of the client mode is that if the client is directly connected with the executors and it is disconnected somehow \(e.g. client on laptop is shutdown\), the entire application will shutdown. As such, the client mode is much more adequate for interactive work.

#### Cluster Mode

Suitable for non-interactive mode. The Client submits the Application, including the Driver, and the Driver will run within the AM itself. In this mode, you actually won't call collect methods but persist the results in some other way \(e.g. store to HDFS\)

#### Settings

| Setting | Definition |
| :--- | :--- |
| --num-executors | How many executors are allocated |
| --executor-memory | RAM per executor |
| --executor-cores | CPU Cores per each executor |



### Notes

This notion of **static partitioning** is not the same as RDDs, what it means is that when you launch your Spark application you will be stuck with the configurations you request. On the other side, with **dynamic partitioning** you can adapt the number of executors in the mid-life of the Spark App.

If the partitions needed to execute a task are persisted within the same machine, the partitions go directly from the executor's JVM heap right to the thread.

On spawn, the driver launches the Spark Web UI web server that allows you to monitor the status of the application. The defaults are 4040 for Spark App UI, 9870 for Resource Manager, 8088 for JobTracker and 8042 for node specific info.

There is a **Scheduler inside the Driver**  that will decide where the actual tasks will be scheduled within the executors. Let's assume we have multiple executors on multiple machines in our cluster. For efficiency purposes, you should assign a Task to the Executor where the partition to be worked on exists \(either cached or in HDFS\). The driver has a **TaskScheduler** that aims to process level locality.

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

## Lifecycle of a Spark App

Every time you execute an **Action** on the application, a **Job** is executed. A Job is composed of one or more **Stages**, which decompose into one or more **Tasks**.

## Memory and Persistence

### Overall Recommendations

* Use at most 75% of machine's memory for Spark
* Minimum executor memory should be 8 GB, Max should be ~40GB \(cf. GC\)

### Persistence

Calling the **cache** method is equivalent to the persist with memory-only `.persist(MEMORY_ONLY)`, but the latter allows for greater flexibility. Partitions cannot be persisted halfway. If the RDD does not fit the size in memory when caching, it won't be stored to disk and it will be recomputed from the last available checkpoint \(previous persist method or read from disk\). The order in which RDDs are evacuated to make room for new cache requests is with the **LRU** \(Least Recent Used\) method.

When using the `.persist(MEMORY_AND_DISK)`, if everything doesn't fit in memory the oldest partition will be moved into the local dir directory \(in its entirety, not partially\). This option keeps the RDDs deserialized in memory but serialized in disk.

A third option `.persist(MEMORY_AND_DISK_SER)` keeps all RDDs serialized both in memory and disk. 

> Persisting stores the RDDs deserialized as Java objects inside the JVM.



