# sbt

## Definition

**sbt** \(simple build tool\) is an interactive build tool for Scala and Java projects, similar to Maven.

**Build automation** is the process of automating the creation of a software build and the associated processes, including compiling the source into binary code, packaging the binary code and running the automated tests.

> **Apache Maven** is a build automation tool used primarily for Java projects, but it can also be used to build an manage projects written in C++, Ruby, Scala and other languages.

## Organization

The **base** or **project's root directory** is the directory holding the project and the `build.sbt`. 

The source files in sbt follow the same directory structure as Maven by default.

```text
// SOURCE CODE STRUCTURE
// other directories in /src and hidden directories will be ignored

src/
    main/
        resources/
        scala/
        java/
    test/
        resources/
        scala/
        java/
```

The **project** folder contains `.scala` files which combined with `.sbt` files form the complete build definition.

## Running SBT

In the terminal, you can navigate to the directory holding the `build.sbt` and run the `$ sbt` command.

```scala
$ sbt        //starts sbt shell
> console    //starts scala REPL
> compile    //compiles the source code under src/main/scala
> test       //runs the unit tests under src/test/scala
> run        //runs the main method (or object extending App)
```

