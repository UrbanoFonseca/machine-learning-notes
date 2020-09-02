# Classes

## Definition

> Classes in Scala are blueprints for creating objects. They can contain methods, values, variables, types, objects, traits, and classes which are collectively called _members_. _Scala Docs_

To create an instance of a class, a `new` keyword is passed along the `type` of the class.

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

