# Scala at Light Speed, Part 4

## FuncProgramming and JVM

Scala runs on the Java Virtual Machine \(JVM\), a VM that enables to run Java and other languages which are also compiled into Java bytecode. Since Java is object-oriented, the JVM knows what an object is but it doesn't recognize functions as first-class citizens.

In **Functional Programming**, we aim to work with functions as we work with any other kind of values. This implies function composition, pass function as arguments and return functions as results.

So, how do we implement functional programming on a JVM that was not developed for it?  By means of **FunctionX**.

