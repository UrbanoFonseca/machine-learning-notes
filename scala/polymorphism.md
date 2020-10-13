# Polymorphism

## Type Parameters

We can define type parameters using the square brackets `[T]`.

Type parameters can be applied to several constructs of Scala, such as Traits, Classes and Functions.

Typically, in Scala type parameters are redundant because the compiler will be able to deduce the correct type parameters from the value arguments of a function call.

## Polymorphism

**Polymorphism** means that a function type comes in many forms. It means that the function can be applied to arguments of many types or that the type can have instances of many types.

There are two main forms of polymorphism: subtyping and generics. 

**Subtyping** means that instances of a subclass can be passed to a base class. 

```scala
case class MyBaseClass(name: String)
class MySubClass extends MyBaseClass(name: String)

def myExampleFunction(ignoreArgument: MySubClass) = println("Print")
```

**Generics** means that we can create many instances of a function or a class by type parameterization. With generics, we can parameterize types with other types.

### Static Binding

**Static binding** is polymorphism applied at compile time.

**Method overloading** enables multiple methods to share the same name but having different signatures, through their data types, order or number of parameters. At compile time, the compiler will know which method to invoke by assessing the type signatures.

### Dynamic Binding

**Dynamic binding** is polymorphism applied at runtime, aka late binding.

**Method overriding** enables a subclass to provide a specific implementation of a method that is already provided by one of its superclasses.

## Bounds

We can define upper and lower bounds for parameter types.

```scala
def assertAll[S <: IntSet](r: S): S = ???
def assertAll[S >: NonEmpty](r: S): S = ???
def assertAll[S >: NonEmpty <: IntSet](r: S): S = ???
```

## Covariance

In Scala, the **arrays are not covariant**, which means that even if a type A is a subtype of B, an Array of A is not considered to be of subtype Array of B.

## Variance

### Lists vs Array

A **List** holds a sequenced, linear list of items and are immutable. Lists in Scala are implemented as Linked Lists. Only sequential access.

An **Array** is flat and mutable. Direct and sequential access.

In rough terms, immutable types can be covariant.

### Definition

Let C\[T\] be a parameterized type, A and B are types such that A &lt;: B. There are 3 possible relationships for C, which are defined during class definition:

| Relationship | Relationship | Class Definition |
| :--- | :--- | :--- |
| Covariant |  ****C\[A\] &lt;: C\[B\]  | class C\[A+\] {..} |
| Contravariant |  ****C\[A\] &gt;: C\[B\] | class C\[A-\] {..} |
| Nonvariant | neither C\[A\] nor C\[B\] is subtypes of the other | class C\[A\] {..} |

