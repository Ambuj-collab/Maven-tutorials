### Differences between pluginManagement and plugins in Maven

* #### Plugin Configuration:
  Maven has two types of plugins:
      
  1) Build – executed during the build process. Examples include Clean, Install, and Surefire plugins. These should be configured in the build section of the POM.
  2) Reporting – executed during site generation to produce various project reports. Examples include Javadoc and Checkstyle plugins. These are configured in the reporting section of the project POM.

For example, we can declare the Jar plugin in the POM:
<pre><code>
&lt;build&gt;
    ....
    &lt;plugins&gt;
        &lt;plugin&gt;
            &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
            &lt;artifactId&gt;maven-jar-plugin&lt;/artifactId&gt;
            &lt;version&gt;3.3.0&lt;/version&gt;
            ....
        &lt;/plugin&gt;
    ....
    &lt;/plugins&gt;
&lt;/build&gt;
</code></pre>
Here, we’ve included the plugin in the build section to add the capability to compile our project into a jar.

* #### Plugin Management:
  In addition to the plugins, we can also declare plugins in the pluginManagement section of the POM. This contains the plugin elements in much the same way as we saw previously. However, by adding the plugin in the pluginManagement section, it becomes available to this POM, and all inheriting child POMs.

  This means that any child POMs will inherit the plugin executions simply by referencing the plugin in their plugins section. All we need to do is add the relevant groupId and artifactId, without having to duplicate the configuration or manage the version.
  
  Similar to the dependency management mechanism, this is particularly useful in multi-module projects as it provides a central location to manage plugin versions and any related configuration.

* #### Example:
  Let’s start by creating a simple multi-module project with two submodules. We’ll include the Build Helper Plugin in the parent POM which contains several small goals to assist with the build lifecycle. In our example, we’ll use it to copy some additional resources to the project output for a child project.

  ###### Parent POM Configuration
  First, we’ll add the plugin to the pluginManagement section of the parent POM:
  <pre><code>
  &lt;pluginManagement&gt;
    &lt;plugins&gt;
        &lt;plugin&gt;
            &lt;groupId&gt;org.codehaus.mojo&lt;/groupId&gt;
            &lt;artifactId&gt;build-helper-maven-plugin&lt;/artifactId&gt;
            &lt;version&gt;3.3.0&lt;/version&gt;
            &lt;executions&gt;
                &lt;execution&gt;
                    &lt;id&gt;add-resource&lt;/id&gt;
                    &lt;phase&gt;generate-resources&lt;/phase&gt;
                    &lt;goals&gt;
                        &lt;goal&gt;add-resource&lt;/goal&gt;
                    &lt;/goals&gt;
                    &lt;configuration&gt;
                        &lt;resources&gt;
                            &lt;resource&gt;
                                &lt;directory&gt;src/resources&lt;/directory&gt;
                                &lt;targetPath&gt;json&lt;/targetPath&gt;
                            &lt;/resource&gt;
                        &lt;/resources&gt;
                    &lt;/configuration&gt;
                &lt;/execution&gt;
             &lt;/executions&gt;
         &lt;/plugin&gt;
      &lt;/plugins&gt;
  &lt;/pluginManagement&gt;
  </code></pre>

This binds the plugin’s add-resource goal to the generate-resources phase in the default POM lifecycle. We’ve also specified the src/resources directory containing the additional resources. The plugin execution will copy these resources to the target location in the project output, as required.
  
Next, let’s run the maven command to ensure that the configuration is valid and the build is successful:
  ```console
  mvn clean test
  ```
After running this, the target location does not contain the expected resources yet.

###### Child POM Configuration
Now, let’s reference this plugin from the child POM:
<pre><code>
&lt;build&gt;
    &lt;plugins&gt;
        &lt;plugin&gt;
            &lt;groupId&gt;org.codehaus.mojo&lt;/groupId&gt;
            &lt;artifactId&gt;build-helper-maven-plugin&lt;/artifactId&gt;
        &lt;/plugin&gt;
    &lt;/plugins&gt;
&lt;/build&gt;
</code></pre>

Similar to dependency management, we don’t declare the version or any plugin configuration. Instead, child projects inherit these values from the declaration in the parent POM.

Finally, let’s run the build again and see the output:
  ```console
  mvn clean test
  ```

```shell
....
[INFO] --- build-helper-maven-plugin:3.3.0:add-resource (add-resource) @ submodule-1 ---
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ submodule-1 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.

[INFO] Copying 1 resource to json
....
```

Here, the plugin executes during the build but only in the child project, because it is used in the build(&lt;build&gt; &lt;/build&gt;) tag, with the corresponding declaration. As a result, the project output now contains the additional resources from the specified project location, as expected.

We should note that only the parent POM contains the plugin declaration and configuration whilst the child projects just reference this, as needed.

The child projects are free to modify the inherited configuration if required.

#### References:  
* https://www.baeldung.com/maven-plugin-management
