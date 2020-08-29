# Elements of Programming

### Programming Paradigms



### Evaluation Strategies

Under **call-by-name** \(CBN\), the functions recompute the passed-in expression's value every time it is accessed. Under the **call-by-value** \(CBV\), the functions compute passed-in expression's value before calling the function. 

```scala
def something() = {println("calling"); 1}

def callByValue(x: Int) = {
    println("x1 => " + x) // x = 1
    println("x2 => " + x) // x = 1
}

def callByName(x: => Int) = {
    println("x1 => " + x) // x = something()
    println("x2 => " + x) // x = something()
}

scala> callByValue(something())
calling
x1 => 1
x2 => 1

scala> callByName(something())
calling
x1 => 1
calling
x2 => 1

```

As a comparison, Python is neither a CBN or a CBV. More precisely, it is a **call-by-object** \(CBO\), **call-by-object-reference** \(CBOR\) or **call-by-object-sharing** \(CBOS\). Objects passed as arguments can be modified even outside the function scope \(e.g. append to a list\). In Python, a variable is not an alias for a location in memory but rather a binding to a Python object.



```scala
// EXAMPLE
// Mixing CBV and CBN allow to assess boolean conditions with short-circuit evaluation,
// where the second condition (y) is only evaluated if the first Boolean is met.
def and(x: Boolean, y: => Boolean) = if (x) y else false
def or(x: Boolean, y: => Boolean) = if (x) true else y
```



### Value Definitions

The `def` form is a by-name since its hand right side is evaluated on each use.

The `val` form is a by-value which is evaluated at the point of definition itself.

```scala
def square(x: Int) = x * x
def y = square(x)
var x = 2
y         // 4
x = 3     // mutated x
y         // 9
```

