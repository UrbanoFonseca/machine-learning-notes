---
description: Guidelines for writing and reading files with Scala.
---

# I/O

## Read Lines

The Scala Standard Library provides an easy way to read a file by lines in`scala.io`

```scala
import scala.io.Source

val in_file = "inputs.csv"

for (line <- Source.fromFile(in_file).getLine){
    println(line)
}
```

## Write Lines

Scala does not have a native line writer but we can leverage Java's BufferedWriter.

```scala
import java.io._

val out_file = "outputs.csv"

val file = new File(out_file)
val bw = new BufferedWriter(new FileWriter(file))

bw.write("21\n")
bw.write("22\n")
bw.close()
```

