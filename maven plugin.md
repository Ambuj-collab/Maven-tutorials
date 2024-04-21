### What are Maven Plugins?
Maven is actually a plugin execution framework where every task is actually done by plugins. 
Maven Plugins are generally used to −
* create jar file
* create war file
* compile code files
* unit testing of code
* create project documentation
* create project reports

A plugin generally provides a set of goals, which can be executed using the following syntax −
```console
mvn [plugin-name]:[goal-name]
```

For example, a Java project can be compiled with the maven-compiler-plugin's compile-goal by running the following command.
```console
mvn compiler:compile
```
Let's create/edit a pom.xml in C:\MVN\project folder.
```console
<project xmlns = "http://maven.apache.org/POM/4.0.0"
   xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation = "http://maven.apache.org/POM/4.0.0
   http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>
   <groupId>com.companyname.projectgroup</groupId>
   <artifactId>project</artifactId>
   <version>1.0</version>
   <build>
      <plugins>
         <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-antrun-plugin</artifactId>
            <version>1.1</version>
            <executions>
               <execution>
                  <id>id.clean</id>
                  <phase>clean</phase>
                  <goals>
                     <goal>run</goal>
                  </goals>
                  <configuration>
                     <tasks>
                        <echo>clean phase</echo>
                     </tasks>
                  </configuration>
               </execution>     
            </executions>
         </plugin>
      </plugins>
   </build>
</project>
```
Next, open the command console and go to the folder containing pom.xml and execute the following **mvn** command.
```console
C:\MVN\project>mvn clean
```
Maven will start processing and displaying the clean phase of clean life cycle.
```console
C:\MVN>mvn clean
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.companyname.projectgroup:project >----------------
[INFO] Building project 1.0
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ project ---
[INFO] Deleting C:\MVN\target
[INFO]
[INFO] --- maven-antrun-plugin:1.1:run (id.clean) @ project ---
[INFO] Executing tasks
     [echo] clean phase
[INFO] Executed tasks
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.266 s
[INFO] Finished at: 2021-12-13T13:58:10+05:30
[INFO] ------------------------------------------------------------------------

C:\MVN>
```
The above example illustrates the following key concepts −

* Plugins are specified in pom.xml using plugins element.
* Each plugin can have multiple goals.
* You can define phase from where plugin should starts its processing using its phase element. We've used clean phase.
* You can configure tasks to be executed by binding them to goals of plugin. We've bound echo task with run goal of maven-antrun-plugin.
* Maven will then download the plugin if not available in local repository and start its processing.
