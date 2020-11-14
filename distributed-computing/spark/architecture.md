# Architecture

## The Gist

Spark is a distributed computing framework that allows to execute code in parallel jobs across multiple machines within a cluster. Some of the key components are the Driver, Executor, Task, Job and Stage.

The **Driver** is the connection to the user. The driver machine is responsible for maintaining information about the Spark application, responding to the user's commands and orchestrating the work across the executors.

The **Executor** is the _workhorse,_ executing the code and reporting back the status of the code dictated by the Driver. Each executor is assigned a Spark **partition**, ****a chunk of data rows that sits on a physical machine within the cluster but which is separate from hard disk partitions. Each executor has a number of **Slots**, each of which can be assigned a Task**. Tasks** are created by the driver, assigned a partition and assigned to a slot.

The **Job** is the parallelized action, which will be broken into multiple Stages. A **Stage** is a set of ordered steps that, together with its complementary stages, accomplishes a Job.

> The Spark parallelism is achieved by two ways: splitting the work between executors and enabling each executor to work on multiple things.

### Transformations, Actions

Spark is lazy, more like a high-functional procrastinator. As any fellow procrastinator will comprehend, waiting until the last minute to do something will give you much more clarity for the overall process and enables you to understand the big picture right from the get-go. The formal definition of waiting until an action is need to execute a series of operations is called **Lazy Evaluation** and this enables Spark to deploy various optimizations.

Transformations are instructions to operate on data to get what you want. Depending on the full availability of data within the same partition to perform the computation, we define **narrow** transformations when all data is available and can be executed within a single partition; and **wide** transformations that require data residing on other partitions. For narrow transformations, data can be kept partitioned as it originally was but with wide transformations data will need to be redistributed within the cluster. Every time data needs to move between executors we call this a **Shuffle.**

On the other hand, we can define **Actions** as operations which are computed and executed when they are called.

> _"Lazy Transformations lead to Eager Actions" \(Plato, 370 BC\)_

\_\_

 

