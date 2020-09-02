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

### Code Blocks

A **code block** is delimited by `{ .. }` such that definitions inside a block are only visible from within. Definitions of outer blocks are visible inside the block unless they are shadowed, i.e. same name replacement definitions.

### Tail Recursion

If a function calls itself as its last action, the function's stack frame can be reused. This is an iterative process called **tail recursion**. A tail recursion function can execute in constant stack space.

```scala
def gcd(a: Int, b: Int): Int = if (b==0) a else gcd(b, a%b)

def factorial(n: Int): Int = if (n==0) 1 else n * factorial(n-1)
```

> In general, if the last action of a function consists of calling a function \(which may be the same\), one stack frame would be sufficient for both functions. This is called **tail calls**.

As we can see in the examples above, `gcd` ends with a function call \(on itself\) but `factorial` ends with a multiplication operation, so that the stack keeps increasing. Being aware of tail recursion \(specially for very deep recursion operations\) can be crucial to avoid StackOverflow errors. 

One can explicitly require a function to be tail-recursive by using the `@tailrec` annotation.
