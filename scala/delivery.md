---
description: Details for deliverable-based content development.
---

# Delivery

### Docs

Run `sbt:package> doc` to automatically generate documentation based on docstrings provided in the source code.

The HTML will be made available under `target/` directory.



### JARring up

The code you develop in Scala may be executed in a machine with Java alone. To do so, you will need to JAR your package with the following steps:

1. Clean `sbt:my-package> clean` to delete all generated files under target/
2. Compile  `sbt:my-package> compile`to compile main sources under `src/main/{scala/java}` directories
3. Test `sbt:my-package> test`to make sure all of your unit tests are running
4. Package `sbt:my-package> package` to create a jar file
   * Contains the files under `src/main/resources/`
   * Contains the classes compiled under `src/main/{scala/java}`
5. If needed, reload `sbt:my-package> reload` to reload build definitions 

