---
description: Why we need all the "public static void main" stuff.
---

# Main Method

\(Access Modifier\) **public** makes the method publicly available, so that the JVM can call the method from outside the class.

\(Class Method\) **static** defines the method as class related, so that the JVM can invoke it without instantiating the class. Saves waste of memory from declaring the object just to call the main method.

\(Return Type\) **void** specifies that the method does not return anything

\(Name\) **main** defines the name of the method. JVM will search for the main as the starting point of a Java program.

\(Arguments\) **String\[\] args** store the Java command line arguments as an array of type String.



Unlike C/C++, programs in Java do not return an exit code. The Java program is not a process of the OS directly, but the JVM is. There is no direct allocation of resources to the Java program directly, nor the program occupies a place in the process table.



