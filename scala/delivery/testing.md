# Testing

## JUnit

**JUnit** is a unit testing framework for Java programming language but it can be extended to Scala.

To be able to run JUnit, extend your `build.sbt` file with the following dependency.

```scala
libraryDependencies += "com.novocode" % "junit-interface" % "0.11" % Test
```

This will guarantee that the JUnit interface dependency is available to your project. Then, extend your package with a test suite.

```scala
package myawesomepackage

import org.junit._
import org.junit.Assert.assertEquals
import scala.

class TesterSuite {

    @Test `my first test`: Unit = {
        assertEquals(1.414, sqrt(2), delta=0.01)
    }
    
}
```

Once you have defined your test suite, you can run `sbt:myawesomepackage> test` and check the package tests results.

