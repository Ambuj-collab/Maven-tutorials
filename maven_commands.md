### Maven commands

* mvn clean install &nbsp; &nbsp; --> to clean the project first by running the clean lifecycle before the new build
* mvn install &nbsp; &nbsp; --> to build your project
* mvn dependency:tree &nbsp; &nbsp; --> to get the dependency tree of your project
* mvn &lt;PLUGIN&gt;:&lt;GOAL&gt; &nbsp; &nbsp; --> To run a specific goal without executing its entire phase (and the preceding phases)
* mvn help:effective-pom &nbsp; &nbsp; --> In order to avoid redundancies and duplication between modules, we often keep common configurations in the shared parent. However, there can be a challenge if we need to have a custom configuration for a child module without impacting all its siblings. That's where we override the parent plugin configuration. The effective POM is affected by various factors like inheritance, profiles, external settings and so on. In order to see the actual POM, let’s run **mvn help:effective-pom** from the child's pom directory
