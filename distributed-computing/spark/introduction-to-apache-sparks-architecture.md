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

Spark is able to achieve parallelization by **scaling horizontally** by adding more executors and **scaling vertically** by adding more cores to the same executor at the same time.

While executors share machine level-resources, tasks share executor-level resources.

