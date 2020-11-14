# Hadoop

## Hadoop

**Hadoop** is a framework for distributed processing of large datasets across clusters of commodity computers and it includes the modules **HDFS** \(Hadoop Distributed File System\), **MapReduce** \(system for parallel processing\)**,** **YARN** \(framework for job scheduling and resource management\), **Common** \(common utilities\) and **Ozone** \(object store\).

> We can define **Big Data** by 3 \(or 5\) V-properties: Volume, Velocity, Variety \(plus Value, Veracity\).

### SQL vs NoSQL

Different databases exist for different purposes. While **Hadoop** may be optimized for batch processing, **SQL** can be a better alternative for online, fast responses.

| Properties | SQL | NoSQL |
| :--- | :--- | :--- |
| Databases | Relational | Non-Relational |
| Query Language | Structured | Dynamic |
| Schema | Predefined | Dynamic |
| Scalability | Vertical | Horizontal |
| Structure | Table | Document, Key-Value,  Graph, Column-wide |

## Filesystem

A Filesystem has many functions:

* Control how data is stored
* Maintain metadata on files and folders
* Manage permissions and security
* Optimize storage space efficiently.

When loading a data file into HDFS it automatically creates 128Mb-sized blocks \(previosuly 64Mb\), as HDFS takes care of distributing the blocks across the nodes and replicates each block \(default to 3\).

### Benefits of HDFS

* Supports **distributed processing** by means of blocks
* **Handles failures** by means of replication
* Built for **horizontally scalability**
* Cost effective by leveraging commodity hardware

## Terminology

**Datanodes** are nodes where the blocks are physically stored. Each datanode knows the list of blocks it is responsible for but it does not know the origin of the data \(e.g. blocks A, B belong to dataset D\).

**Namenodes** keep track of all files in HDFS. For any given file, it knows the list of blocks that make up the file as well as their location. They also keep metadata for files and folders on hdfs \(e.g. replication factor, permissions\). Under the hood, they store FsImage and EditLogs in disk and block locations \(info on Datanodes\) in memory.

**FsImage** is a OS-filesystem stored file that contains the directory structure.

**EditLogs** is a transaction log that tracks changes to HDFS since the last FsImage was created.

## Configuration

On the `/etc/hadoop/conf` directory, we have available the `core-site.xml` file which is made available to all nodes in the cluster. In it, the property `fs.defaultFS` specifies the location of the name node.

The `hdfs-site.xml` list all the properties specific to hdfs, such as:  
\* `dfs.namenode.name.dir` with the location in local FS where namenode can store its files  
\* `dfs.datanode.data.dir` with the location in local FS where datanode can store its files and blocks  
\* `dfs.namenode.http-address` with the http address of name node  
\* `dfs.replication` for the replication factor of the blocks. default = 3

The **Hadoop Cluster** node is composed of CPU, RAM and Disk. The **namenodes** will be heavier and RAM and the **datanodes** will be heavier in disk.

> A **Rack** is a group of nodes connected in a network where a **Cluster** is a group of racks connected in a network.



