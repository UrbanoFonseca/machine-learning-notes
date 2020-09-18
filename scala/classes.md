# Classes

## Definition

> Classes in Scala are blueprints for creating objects. They can contain methods, values, variables, types, objects, traits, and classes which are collectively called _members_. _Scala Docs_

To create an instance of a class, a `new` keyword is passed along the `type` of the class \(similar to C++, Java\).

```scala
class Rational(x: Int, y: Int) {
    def numer = x
    def denom = y
}

val r = new Rational(2, 3)
```

With the above definition, we define:

1.  new type called`Rational`
2. constructor `Rational` to create elements of such type

Scala stores names of types and values in different namespaces, so there is no conflict between the two definitions.

### Self-Reference

On the inside of a class, the name `this` represents the object on which the current method is executed.

### Preconditions

We can enforce predefined conditions with requirements made available by the predefined `require` function, in which we specify the boolean condition for the requirement and an optional corresponding `IllegalArgumentException` string that is thrown when the condition evaluates to false.

```scala
class Rational(x: Int, y: Int) {
  require(y != 0, "Denominator must be nonzero")
}

new Rational(2, 0) // throws IllegalArgumentException
new Rational(2, 1) // Passes the requirements
```

Alternatively, we can leverage the predefined `assert` function which also takes on a condition and an optional message string. If the condition is not met, this will raise an `AssertionError` instead of an `IllegalArgumentException` since they are meant for different purposes.

> `require` is used to enforce a precondition on the caller of a function while `assert` is used to check the code of the function itself.

### Constructors

A class implicitly introduces a constructor. The class definition is also the constructor definition. The **primary constructor** of a class:

1. takes the parameters of the class, and
2. executes all statements in the class body

We can extend the standard constructor of a class to use partially-defined arguments.

```scala
class Rational(x: Int, y: Int) {
//    def this(x:Int) = this(x, 1)
}

// These will generate a similar object.
new Rational(3, 1)
new Rational(3)
```

Bear in mind that constructor arguments are not fields unless specified by a `val` keyword! After you construct a class with a constructor argument, that argument is ephemeral and not visible outside the class definition.

```scala
class Dog(val name: String, age:Int=0) {}

val goodBoy = new Dog("Rufus")
goodBoy.age // error. value age is not a member of Dog
goodBoy.name // Rufus
```

## Objects

Elements of a class are called **objects.** These are created by prefixing an application of the constructor of the class with the keyword `new` .

### Singleton Objects

A **Singleton Object** is a class that has exactly one instance and it is created lazily when it is referenced.

A singleton object named the same as a class is called a **companion object** and it must be defined in the same source file as the class.

A singleton object can extend an abstract class and this is used when it makes sense just to have one instance of a specific subclass.

> Singleton objects are values, so a singleton object evaluates to itself.

### Object Extends App

When you write `object Something extends App { }`, it means that what you write between those brackets is executable as a standalone App.

## Data Abstraction

The ability to choose from different implementations of the data without affecting the desired behaviour is called **data abstraction**. In Scala, we can think of deciding whether to calculate some value as `val` or leave that as a `def` depending on how many calls we expect to happen. 

> **Data abstraction** is a programming and design technique that relies on the separation of interface and implementation and it refers to providing only essential information to the outside world while hiding their background details. _\(Tutorials Point, Data Abstraction in C++\)_

## Operators

> An **operator** is a symbol that represents an operation to be performed with one or mode operand.

### Precedence

The precedence of an operator is determined by its first character, according to the following list.

```scala
(characters not shown below)
* / %
+ -
:
= !
< >
&
^
|
(all letters)

// Example
a + b ^? c ?^ d less a ==> b | c             // What is the order here?
a + b ^? (c ?^ d) less a ==> b | c           // 1. ? character
(a + b) ^? (c ?^ d) less a ==> b | c         // 2. + operator
(a + b) ^? (c ?^ d) less (a ==> b) | c       // 3. ==> starts with '=' character
((a + b) ^? (c ?^ d)) less (a ==> b) | c     // 4. ^? starts with '^'
((a + b) ^? (c ?^ d)) less ((a ==> b) | c)   // 5. | operator
((a + b) ^? (c ?^ d)) less ((a ==> b) | c)   // 6. less starts with a letter
```

### Infix Notation

Any method with a parameter can be used as an infix operator.

```scala
r.add(s)  // add r to s
r add s   // add r to s
```

### Relaxed Identifiers

Operators can be used as identifiers. Thus, identifiers can be **alphanumeric**, starting with a letter followed by a sequence of letters or numbers, or **symbolic**, starting with an operator symbol followed by other operator symbols.

### Unary Operators

You can specify 4 unary prefixes `unary_-`, `unary_+`, `unary_!` and `unary_~` that extend the functionality of a class.

```scala
class Rational(x: Int, y: Int) {
    def numer = x
    def denom = y
    def unary_- = new Rational(-x, y)
}

// Same thing
val x = new Rational(2, 3)
(-x).numer // -2
(-x).denom // 3
-x.denom // -3 . Why? Because the minus is applied to x.denom
```

## Inheritance

Inheritance works by extending the fields and methods of a class by the class it extends.

```scala
class Animal {
  val age: Int = 0
  def eat() = println("I'm eating")
}

class Dog extends Animal

val rufus = new Dog
rufus.eat() // prints "I'm eating"
```

## Case Class

**Case classes** are lightweight data structures with boilerplate for some functionalitites, such as equals and hash code, serialization and pattern matching.

```scala
case class Bass(strings: Int, name: String)

val fenderJazz = Bass(4, "Fender Jazz") // same as Bass.apply(...)
val duplicateFenderJazz = Bass(4, "Fender Jazz")
fenderJazz equals duplicateFenderJazz // true. equals is based on values.
```

## Apply

The presence of an `apply` method allows an instance of a class to be called as a function.

```scala
class Bass(brand: String) {
  def apply(strings: Int) = println(s"My $brand bass has $strings strings")
}

val fenderJazz = new Bass("Fender")
fenderJazz.apply(4)
fenderJazz(4)
```

## Abstract Classes

Similar to Java's **abstract classes,** Scala implements the usage of an `abstract class` when you either want to create a base class that requires constructor arguments or if your scala code will be called from Java code.

Since **Traits** do not allow for constructor parameters, if you need your base behaviour to depend on the input at instantiation you will need the abstract class. Bear in mind that a class can only extend one abstract class.

In Scala, any user-defined class extends another class. If no superclass is defined, the standard class `Object` in the package `java.lang` is assumed. The direct or indirect superclasses of a class C are called **base classes** of C. If we define a superclass `Bass` and a subclass `JazzBass`, the base classes of `JazzBass` are both `Bass` and `Object`.



