# Traits

## Definition

A **Trait** is a way to define method and field definitions such that a Class that extends the Trait must provide implementations of its definitions. A Scala Trait is similar in spirit to the Java Interface, except they can contain fields and concrete methods. Nonetheless, Traits cannot have parameters, only classes can.

A Trait can have both abstract and concrete implementations defined.

```scala
trait Bass {
  def strings: Int
  def frets: Int
  def notes = strings * frets
}

trait Instrument {
  def describe = println("I am a musical instrument")
}

case class JazzBass(strings: Int, frets: Int) extends Bass with Instrument

val fenderJazz = new JazzBass(4, 12)
fenderJazz.notes // returns 48
fenderJazz.describe // prints the message
```

### Inheritance vs Extension

The difference between Class Inheritance \(Class inherits another Class\) and the Extension with Traits is that while a Class can only inherit from just one superclass, it can extend multiple Traits.

> A Class can inherit 1 Class \(single inheritance\) and extend N Traits.



