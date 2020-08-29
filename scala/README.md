# Scala

## Source Files, Class Files, JVM

Source code is stored in files with `.scala` extension. Ideally, one source file is used for either each class or each class hierarchy. Same source file can define multiple classes and objects. For an intuitive structure of code, each source file should be named after the name of the class it defines and package hierarchies should be reflected in the directory structure.

The `.scala` files are compiled into `.class` Java class files.  
CLASS files are binary files containing bytecode executable by the Java Virtual Machine \(JVM\) and are usually bundled into a `.JAR` files, which are included in the `$CLASSPATH` environment variable for execution. Class files can be compiled using the `javac`command.  
A JAR file is a Java Archive file used by the Java Runtime Environment \(JRE\), a framework for executing Java programs. JAR files are compressed with .zip compression 



### Java - JDK, JRE & JVM

The **Java Development Kit** \(JDK\) is a software development environment used for developing Java applications. It includes the JRE, an interpreter/loader \(Java\), a compiler \(javac\), an archiver \(jar\), a document generator \(javadoc\) and other tools for Java development.

The **Java Runtime Environment** \(JRE\) provides the minimum requirements for executing a Java application, it consists of a JVM, core classes and supporting files.

The **Java Virtual Machine** \(JVM\) is responsible for executing the Java program line by line. 

JDK = JRE + Development Tools

JRE = JVM + Library Classes



