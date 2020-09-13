# Scala

## Source Files, Class Files, JVM

Source code is stored in files with `.scala` extension. Ideally, one source file is used for either each class or each class hierarchy. Same source file can define multiple classes and objects. For an intuitive structure of code, each source file should be named after the name of the class it defines and package hierarchies should be reflected in the directory structure.

The `.scala` files are compiled into `.class` Java class files.  
CLASS files are binary files containing bytecode executable by the Java Virtual Machine \(JVM\) and are usually bundled into a `.JAR` files, which are included in the `$CLASSPATH` environment variable for execution. Class files can be compiled using the `javac`command.





## Expressions

In Scala, everything is an expression. Instead of thinking about instructions, i.e. things that the computer does sequentially, we think about values and composing values into new values.

> An **expression** is a structure that can be reduced into a value

## Unit Type

In Scala, the **Unit Type** is associated with "no meaningful value", similar to `void` in other languages.

