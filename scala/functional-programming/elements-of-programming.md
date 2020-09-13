# Elements of Programming

## Programming Paradigms

Paradigms are ways to classify programming languages based on their features. The same language can be classified under multiple paradigms.

Common paradigms include:  
- **Imperative** where the programmer instructs how to change state  
- **Declarative** where the programmer declares properties of the desired result without specifying how to compute it.

Imperative programming includes the **procedural** paradigm, which groups instructions into procedures/routines \(e.g. FORTRAN, COBOL, C\), and ****the **object-oriented** paradigm, which groups instructions together with the part of the state they operate upon \(e.g. Java, Python, Scala\).

Declarative programming includes the **functional** paradigm, in which the result is declared as the value of a series of function applications \(e.g. Python, Kotlin, Scala\), the **logic** paradigm, in which the desired result is declared as as the answer to a question about a system of facts and rules \(e.g. Prolog\), and the **mathematical** paradigm, in which the result is declared as a solution of an  optimization problem.

## Evaluation Strategies

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

## Value Definitions

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

## Code Blocks

A **code block** is delimited by `{ .. }` such that definitions inside a block are only visible from within. Definitions of outer blocks are visible inside the block unless they are shadowed, i.e. same name replacement definitions.

## Tail Recursion

If a function calls itself as its last action, the function's stack frame can be reused. This is an iterative process called **tail recursion**. A tail recursion function can execute in constant stack space.

```scala
def gcd(a: Int, b: Int): Int = if (b==0) a else gcd(b, a%b)

def factorial(n: Int): Int = if (n==0) 1 else n * factorial(n-1)
```

> In general, if the last action of a function consists of calling a function \(which may be the same\), one stack frame would be sufficient for both functions. This is designated as **tail calls**.

As we can see in the examples above, `gcd` ends with a function call \(on itself\) but `factorial` ends with a multiplication operation, so that the stack keeps increasing. Being aware of tail recursion \(specially for very deep recursion operations\) can be crucial to avoid StackOverflow errors. 

One can explicitly require a function to be tail-recursive by using the `@tailrec` annotation.

> In Scala, instead of using loops and iteration we use **recursion** by definition.

### The Magic Trick

Detailing the tail recursion mechanism is key to understand its impact on memory requirements and efficiency.

> The magic trick with tail recursion is that if the function will only call another function, then everything in the past can be erased and you only need to care about that new function with those arguments.

Let's take the Factorial example in its two versions: tailed- and non-tailed-recursive implementations.

```scala
// Factorial
// Multiply ints from 1 to n
// 3! = 6 = 3 * 2 * 1
// 5! = 120 = 5 * 4 * 3 * 2 * 1

def tailedFib(n: Int, acc: Int = 1): Int = {
  if (n==1) acc
  else tailedFib(n-1, acc * n)
}

/** tailed implementation only needs to keep a constant number of elements
 in memory during the calculation.

tailedFib(3)
tailedFib(2, 3)
tailedFib(1, 6)
6
*/
  
  
def nonTailedFib(n: Int): Int = {
  if (n==1) 1
  else n * nonTailedFib(n-1)
}  
/** 
nonTailed Implementation needs to keep an increasing number of elements until
it evaluates the final result.

nonTailedFib(3) = 
3 * nonTailedFib(2)
3 * 2 * nonTailedFib(1)
3 * 2 * 1
6
*/ 

```

