---
description: Notes on the Databricks course.
---

# Introduction to Apache Spark's Architecture

## Filtering

### Slots vs Threads vs Cores

Often when we talk about details of the executors we talk about these 3 different concepts, which are not interchangeable _per se_. A **Slot** is a place in which the driver can assign a task, the **Cores** refer to the hardware and **Threads** refer to thread pulling. 

A **Cluster** is a group of **Nodes**, each being able to host one or more Executors \(i.e. JVMS\). If node is shared, the executors will be competing for the same physical resources \(e.g. RAM, disk\).

_Example: On a big node with 8 cores, one cannot have 3 executors each requesting 4 cores each otherwise it will incur in thread oversubscription. To achieve maximum parallelization, the \#tasks should match the \#executors \* \#cores \(per executor\)._

In Spark, once a partition is set you will never subdivide it. A Task will never process _less-than_ the full partition itself.

The Application is the code submitted to the driver using Spark submit, which can include 1 or more Jobs.

### App, Job, Stage, Task

An **Application** contains zero or more jobs. A **Job** contains one or more stages. A **Stage** contains one or more tasks**.** A **Tasks** is the work against a partition. 

> One Task, One Partition. \(Confucius, 500 BC\)

One Spark Action can require one or more jobs, the number of jobs and stages being ditacted by the driver.

Spark is able to achieve parallelization by **scaling horizontally** by adding more executors and **scaling vertically** by adding more cores to the same executor at the same time.

While executors share machine level-resources, tasks share executor-level resources.

An **Executor** is an instance of a JVM running an app ready to receive instructions from a driver, leveraging the JVM for running many threads and providing the environment to run the tasks.

## Counting

### Stages

In the case of a given task is stuck or taking too much time to compute, the driver may have two options: it either assigns the full task to other slot or it asks a second opinion and assumes the result from whoever finishes first. The technique of **speculative execution** is defined by actively requiring an extra working slot to execute the exactly same task as the one the driver suspects it will take too long \(e.g. heartbeat failure\). The upside is that we increase robustness by decreasing the long-tails of way-to-much time to execute on some instances but we require more resources on the flipside.

**Shuffle data** is data that is persisted to disk by the tasks on execution that is later read on other stages.

### Local and Global Count

Let's take the example of a simple operation during the lifetime of a Data Scientist: counting how many rows the dataset contains. Since the dataset is distributed into multiple partitions, we will have two main operations: many local counts and one global count. On the first stage, each task counts the number of records contained in the partition that is associated with and persists the result to disk. Later, when every task is complete, a shuffle operation reads those intermediate results and sends all the local counts into one slot/task in one of the executors that will be responsible for summing up those intermediate results and generate the global answer, which it sends to the driver.

In this case, the Job would be counting the rows; Stage 1 would be producing the local count in which its tasks would be count the rows in each partition; Stage 2 would be producing the global count in which its only task would be to sum up the intermediate local counts.

### Recap

A Stage cannot be completed until all tasks are completed. On Spark 3.x, some stages can be ran in parallel \(e.g. deciding on broadcast vs merge sort\).

## Shuffling

### Count Distinct

With a common _countDistinct_ operation, we can observe how Spark splits this job into stages. In the first Stage, Spark will perform a local distinct, where each slot creates a set of unique distinct values available within the partition assigned to it. When that is completed, the Spark task will write shuffle files \(disk I/O\) in that executor and wait until everybody \(all the other tasks\) are ready - **Stage Boundry.** On the second stage, each slot on the executors will have some unique values assigned to it and it will fetch from the remaining executors hard drives' \(disk + network I/O\)  the remaining values associated to the ones it is responsible for. 

### Operations

A **Count Distinct** operation occurs when you want to calculate the number of uniquely distinct values from a set. A **Cross Join** occurs when you create all the combinations between rows of different datasets \(_e.g TableA = \[A, B\], TableB=\[1, 2, 3\] =&gt; crossJoin\(A,B\) =\[A1, A2, A3, B1, B2, B3\]\)._

A **Union** occurs when you combine data into new rows, the first set of rows is extended with the latter and the column structure stays the same. A **Join** occurs when you combine data into new columns, the same set of rows is enriched with additional information for the same entry and the column structure \(schema\) changes. _While a Union is a narrow operation, a Join is a Wide operation._

With **Coalesce,** you reduce the number of partitions of a dataset. With **Repartition,** you can either increase or decrease the number of partitions. _A Coalesce is narrow but repartitions are wide._

A **Filter** enables you to subset the data according to specified criteria. A **Map** is a unitary transformation applied on every element. 

> **Shuffling** is the process of rearranging data within a cluster between stages and it is triggered by wide operations.

Depending on the nature of the operations, they can be considered **narrow -** can be aggregated with local computations only on each partition - or **wide** - require sharing of data between multiple executors.

For a better use of the Catalyst Optimizer, aim to group wide transformations together instead of going back and forth between wide and narrow operations. ****





