# MapReduce

## Intro

**MapReduce** is a programming model for distributed computing consisting on three phases:

1. **Map:** InputSplitters, Mappers, Key-Value Pairs;
2. **Shuffle**: values from different mappers are transferred to reducers;
3. **Reduce:** Reducers \(consolidate result from multiple mappers\), Result

## Phases

### **Map**

**Input Splits** are chunks of data which are later processed by mappers. the mapper will execute the same set of code on every single record, outputting a key-value pair. The input split is not the same as blocks, it is not some chunks of data. In fact, `Input Split`is a Java class that points to the start and end location within blocks, where the start can of the split can start in a block and end in another block.

A **Mapper** can be written in many programming languages. The number of mapppers generated are the same as the number of input splits.

In the **Map Phase**, each key is assigned to partitions by a class called `Partitioner`, where the number of partitions equals the number of reducers. Each partition is copieed to the appropriate reducers. The reduce only occurs after every partition is merged. During the Map phase, we can have an optional **Combiner** which is used to reduce the amount of data that is sent to the reducers. Combiner is like a mini reducer that runs during the Map phase.

The process of a Mapper is as follows:

* Dataset is divided into multiple parts - InputSplits
* Each mapper processes an IS
* Each mapper can be called multiple times depending on the content of the IS
* Mapper will emit a key-value pair as output
* There is 1+mappers in a MapReduce job, defined by the number of IS

### Shuffle

During the **Shuffle Phase**, the mapper outputs are sorted, copied \(i.e. assigned to reducers\) and merged. 

During the **Shuffle Phase**, the mapper outputs are sorted, copied \(i.e. assigned to reducers\) and merged. All key-value pairs of the Map stage for some entity should go to the same reducer.

### Reduce

A **Reducer** will receive one record per each entity \(i.e. key\) and the number of reducers can be set by the user.

The process of a Reducer is:

* Reduce functions will take key-value pairs from multiple Map functions as inputs and Reduce them to an output
* Keys are grouped with values. The Reduce function is called once per each key and its values.
* There can be 0, 1 or more Reduce functions for a MapReduce job.

### Overall

For a **MapReduce job** to run, you need to specify its inputs, outputs, the Mapper and the Reducer Tasks.

Additionally, the **InputFormat** class validates the inputs, splits files into logical InputSplits and provides a RecordReader implementation to extract the logical records. The **OutputFormat** validates the output specification and provides a RecordWriter implementation to write output files of the job.

## Example

```java
Job job = new Job();

job.setJarByClass(SomeJar.class);
job.setJobName("Intro to MapReduce");

FileInputFormat.addInputPath(..);
FileOutputFormat.addOutputPath(..);

job.setInputFormatClass(TextInputFormat.class);
job.setOuptutFormatClass(TextOutputFormat.class);

job.setMapperClass(SomeMapper.class);
job.setReducerClass(someReducer.class);

job.setOutputKeyClass(Text.class);
job.setOutputValueClass(FloatWritable.class);

System.exit(job.waitForCompetion(true) ? 0 : 1);
```

## **Serialization**

**Writable** is a serializable object which implements a simple, efficient serialization protocol.   
_But why don't we use native Java types?_

Since Hadoop is used for distributed computing, data must be transferred in the network and the object converted into a byte stream, a process known as serialization. Java serialization is not effective in speed and size.

**Java Serialization** writes the class name of each object being serialized into the bytestream to know the object's type on deserialization. So each subsequent instance of the class must have the reference to the first instance.

The first problem is the space, as you are repeatedly transmitting the same class name at each instance. The second is that the reference handles \(which link subsequent instances to the first of same class\) introduce a problem during sorting records in a serialized stream. If you don't write the type to avoid such problems, the assumption is that the client knows the type.

Another benefit is that with native Java types, when the object is being reconstructed in the serialization process a new instance for each object must be created, where with writables the same object can be re-used.

