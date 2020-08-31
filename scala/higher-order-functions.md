# Higher Order Functions

### Definition

Functional languages treat functions as _first-class citizens,_ which means that they can be passed as a parameter and returned as a result_._ Functions that take other functions as parameters or that return functions as results are called **higher order functions**.

> In programming languages, **first-class citizens** is an entity which supports all types of operations generally available to other entities. These operations typically include being passed as an argument, returned from a function, modified and assigned to a variable.

The **function type** is `A => B` takes an argument of type A and returns a result of type B.

**Anonymous functions** are function literals, which allow to write a function without specifying a name. As an example, a square function would be `(x: Int) => x * x`. Additionally, the type parameter can be omitted if it can be inferred by the compiler.

### Currying

**Currying** can be defined as a style of defining a function so that it is mapped to an expression that consists of nested anonymous functions that take one argument.

> **Currying** is the process of taking a function that takes multiple arguments in a tuple as its argument, into a function that takes just a single argument and returns another function which accepts further arguments, one by one, that the original function would receive in the rest of its tuple \(Haskell Wiki\)

### Fixed Points

A number x is called a **fixed point** if `f(x) = x` . Some functions allow to locate fixed points in an iterative way by starting with an initial estimate and applying f\(x\) iteratively, such that `f(x), f(f(x), f(f(f(x), ...`, until the value does not change anymore.  




