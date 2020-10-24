# Collections

## Maps

Maps are iterables that allow to associate Keys and Values.

The method `apply` will be an all-or-nothing option that if the element to be retrieved does not exist, it will raise an Exception. On the contrary, the `get` method allows for a graceful return with the Option Type, returning either None or Some\(x\).

```scala
// bestSongs : Map[String, String]
def playTune(music: String) = bestSongs.get(music) match {
  case None => "That songs is not that good :("
  case Some(s) => s
}
```

### Default Values

The operation `withDefaultValue` allows to return something when the Map tries to fetch a non-existing element.

## Option Type

```scala
trait Option[+A]
case class Some[+A](value: A) extends Option[A]
object None extends Option[Nothing]
```

## SQL Queries

There are 2 relevant operations worth to mention: `orderBy` and `groupBy`.

The **orderBy** method allows you to sort base on either a `sortWith` or a `sorted`.

The **groupBy** associates a given discriminator function, returning a Map of each possible return value along with a list of all the elements that have matched such discriminator function.

