### Maven Dependency Scopes

Dependency scopes can help to limit the transitivity of the dependencies. They also modify the classpath for different build tasks. Maven has six default dependency scopes.

* compile: This is the default scope when no other scope is provided. Dependencies with this scope are available on the classpath of the project in all build tasks. They are also propagated to the dependent projects.
* provided: We use this scope to mark dependencies that should be provided at runtime by JDK or a container. A good use case for this scope would be a web application deployed in some container, where the container already provides some libraries itself. For example, this could be a web server that already provides the Servlet API at runtime. In our project, we can define those dependencies with the provided scope:
  <pre><code>
  &lt;dependency&gt;
    &lt;groupId&gt;jakarta.servlet&lt;/groupId&gt;
    &lt;artifactId&gt;jakarta.servlet-api&lt;/artifactId&gt;
    &lt;version&gt;6.0.0&lt;/version&gt;
    &lt;scope&gt;provided&lt;/scope&gt;
  &lt;/dependency&gt;
  </code></pre>
  The provided dependencies are available only at compile time and in the test classpath of the project. In other words, a dependency with this scope is added to the classpath used for compilation and test, but not the runtime classpath
* runtime: This scope indicates that the dependency is not required for compilation, but is for execution. Maven includes a dependency with this scope in the runtime and test classpaths, but not the compile classpath.
* test: We use this scope to indicate that dependency isn’t required at standard runtime of the application but is used only for test purposes. In other words, this scope indicates that the dependency is not required for normal use of the application, and is only available for the test compilation and execution phases.
  The standard use case for this scope is adding a test library such as JUnit to our application:
  <pre><code>
  &lt;dependency&gt;
    &lt;groupId&gt;junit&lt;/groupId&gt;
    &lt;artifactId&gt;junit&lt;/artifactId&gt;
    &lt;version&gt;4.12&lt;/version&gt;
    &lt;scope&gt;test&lt;/scope&gt;
  &lt;/dependency&gt;
  </code></pre>
* system: System scope is very similar to the provided scope. The main difference is that system requires us to directly point to a specific jar on the system. It’s worthwhile to mention that system scope is deprecated. The important thing to remember is that building the project with system scope dependencies may fail on different machines if dependencies aren’t present or are located in a different place than the one systemPath points to:
  <pre><code>
  &lt;dependency&gt;
    &lt;groupId&gt;com.baeldung&lt;/groupId&gt;
    &lt;artifactId&gt;custom-dependency&lt;/artifactId&gt;
    &lt;version&gt;1.3.2&lt;/version&gt;
    &lt;scope&gt;system&lt;/scope&gt;
    &lt;systemPath&gt;${project.basedir}/libs/custom-dependency-1.3.2.jar&lt;/systemPath&gt;
  &lt;/dependency&gt;
  </code></pre>
* import: It’s only available for the dependency type pom. import indicates that this dependency should be replaced with all effective dependencies declared in its POM. Here, below custom-project dependency will be replaced with all dependencies declared in custom-project’s pom.xml <dependencyManagement> section.
  <pre><code>
  &lt;dependency&gt;
    &lt;groupId&gt;com.baeldung&lt;/groupId&gt;
    &lt;artifactId&gt;custom-project&lt;/artifactId&gt;
    &lt;version&gt;1.3.2&lt;/version&gt;
    &lt;type&gt;pom&lt;/type&gt;
    &lt;scope&gt;import&lt;/scope&gt;
  &lt;/dependency&gt;
  </code></pre>

  **What is "pom" packaging in maven?**
  pom packaging is simply a specification that states the primary artifact is not a war or jar, but the pom.xml itself. Often it is used in conjunction with "modules" which are typically contained in sub-directories of the project(Multi-Module Maven Project), however, it may also be used in certain scenarios where no primary binary was meant to be built, all the other important artifacts have been declared as secondary artifacts.
  Among all packaging types, pom is the simplest one. It helps to create aggregators and parent projects. An aggregator or multi-module project assembles submodules coming from different sources. These submodules are regular Maven projects and follow their own build lifecycles. The aggregator POM has all the references of submodules under the modules(&lt;modules&gt; &lt;/modules&gt;) element. A parent project allows you to define the inheritance relationship between POMs.  The parent POM shares certain configurations, plugins, and dependencies, along with their versions. Most elements from the parent are inherited by its children — exceptions include artifactId, name, and prerequisites. Because there are no resources to process and no code to compile or test, hence, the artifacts of pom projects generate themselves instead of any executable.

Let’s define the packaging type of a multi-module project:
```console
<packaging>pom</packaging>
```

#### References:  
  1) https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html
  2) https://www.baeldung.com/maven-dependency-scopes
