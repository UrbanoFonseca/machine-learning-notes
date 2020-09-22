# Scala at Light Speed, Part 4

## FuncProgramming and JVM

Scala runs on the Java Virtual Machine \(JVM\), a VM that enables to run Java and other languages which are also compiled into Java bytecode. Since Java is object-oriented, the JVM knows what an object is but it doesn't recognize functions as first-class citizens.

In **Functional Programming**, we aim to work with functions as we work with any other kind of values. This implies function composition, pass function as arguments and return functions as results.

So, how do we implement functional programming on a JVM that was not developed for it?  By means of **FunctionX**.

> All Scala functions are instances of the FunctionX type.

## FunctionX

Scala provides 23 traits that are used for defining functions from 0 up to 22 arguments \(something that will be dropped in Scala 3 aka Dotty\). This 22-arguments limitation occurs because functions need to be  rewritten as objects and only those traits are implemented.

Thus, when defining a function we are basically creating new instances of those FunctionX traits.

```scala
// The Type Parameters are for arguments type and return type
val simpleIncrementer = new Function1[Int, Int] {
  override def apply(arg: Int): Int = arg + 1
}

simpleIncrementer(20) // 21

val stringConcatenator = new Function2[String, String, String] {
  override def apply(arg1: String, arg2: String): String = arg1 + arg2
}

stringConcatenator("This", "That") // "ThisThat"

val defaultImplementation = new Function0[String] { 
  override def apply = "This is very cool"
}

defaultImplementation() // Returns the desired string
```

**Breakdown:**

1. Traits implement restrictions on what the implementation should provide
2. The `apply` method allows calling an object with an argument
3. After instantiating with the `new` keyword, the object now allows to create an UX which is function-oriented.

## **Collections**

### **List**

```scala
// Create a list
val aList = List(1, 2, 3)

// Get Elements
val firstElement = aList.head // head - first element
val rest = aList.tail         // tail - remaining values in a list

// Prepend, Append
val aPrependedList = 0 :: aList // :: prepend operator. List(0, 1, 2, 3)
// +: prepends, :+ extends
val anExtendedList = 0 +: aList :+ 6 // List(0, 1, 2, 3, 6)
```

### **Sequences, Vectors**

A **Sequence** denoted by `Seq` keyword has a Trait \(e.g. abstract type\) and an `apply` factory method. The main characteristic of Sequences is that you can get the value at a given index.

A **Vector** is particular type of Sequence which is very fast for large data. 

```scala
val aSequence: Seq[Int] = Seq(1, 2, 3)
val accessedElement = aSequence(1) // returns 2, due to 0-index

// Vector
val aVector = Vector(1, 2, 3)
```

### **Set**

A set is a collection with no duplicates.

```scala
val aSet = Set(1, 4, 2, 3, 2) // Set(1, 4, 2, 3)
val setHasFive = aSet.contains(5) // false
val anAddedSet = aSet + 5 // Set(5, 1, 4, 2, 3)
val aRemovedSet = aSet - 3 // Set(1, 4, 2)
```

### **Ranges**

A **Range** is an ordered sequence of integers that are equally spaced apart, useful for iterations. 

```scala
val aRange = 1 to 1000
val twoByTwo = aRange.map( x => x * 2).toList
```

### **Tuples**

**Tuples** are groups of values under the same value.

```scala
val aTuple = Tuple("Fender", "Jazz", 4)
```

### **Maps**

A **Map** is an association between keys and values, similar to dictionary.

```scala
val aCatalog: Map[String, Int] = Map(
  ("Apples", 50),
  "Oranges" -> 100 // similar representation
)

val priceOfApples = aCatalog("Apples") // 50
```

