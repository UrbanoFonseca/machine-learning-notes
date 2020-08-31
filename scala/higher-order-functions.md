# Higher Order Functions

### Definition

Functional languages treat functions as _first-class citizens,_ which means that they can be passed as a parameter and returned as a result_._ Functions that take other functions as parameters or that return functions as results are called **higher order functions**.

> In programming languages, **first-class citizens** is an entity which supports all types of operations generally available to other entities. These operations typically include being passed as an argument, returned from a function, modified and assigned to a variable.

The **function type** is `A => B` takes an argument of type A and returns a result of type B.

**Anonymous functions** are function literals, which allow to write a function without specifying a name. As an example, a square function would be `(x: Int) => x * x`. Additionally, the type parameter can be omitted if it can be inferred by the compiler.



