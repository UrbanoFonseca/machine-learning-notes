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

**Generics** means that we can create many instances of a function or a class by type parameterization.

### Static Binding

**Static binding** is polymorphism applied at compile time.

**Method overloading** enables multiple methods to share the same name but having different signatures, through their data types, order or number of parameters. At compile time, the compiler will know which method to invoke by assessing the type signatures.

### Dynamic Binding

**Dynamic binding** is polymorphism applied at runtime, aka late binding.

**Method overriding** enables a subclass to provide a specific implementation of a method that is already provided by one of its superclasses.

