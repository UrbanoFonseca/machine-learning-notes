# Polymorphism

## Type Parameters

We can define type parameters using the square brackets `[T]`.

Type parameters can be applied to several constructs of Scala, such as Traits, Classes and Functions.

Tipically, in Scala type parameters are redundant because the compiler will be able to deduce the correct type parameters from the value arguments of a function call.

## Polymorphism

**Polymorphism** means that a function type comes in many forms. It means that the function can be applied to arguments of many types or that the type can have instances of many types.

There are two main forms of polymorphism: subtyping and generics. **Subtyping** means that instances of a subclass can be passed to a base class. **Generics** means that we can create many instances of a function or a class by type parameterization.

