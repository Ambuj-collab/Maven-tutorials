### Differences between dependencyManagement and dependencies in Maven

In the parent POM, the main difference between the &lt;dependencies&gt; and &lt;dependencyManagement&gt; is this:
* Artifacts specified in the &lt;dependencies&gt; section will ALWAYS be included as a dependency of the child module(s).
* Artifacts specified in the &lt;dependencyManagement&gt; section, will only be included in the child module if they were also specified in the &lt;dependencies&gt; section of the child module itself. Why is it good you ask? Because you specify the version and/or scope in the parent, and you can leave them out when specifying the dependencies in the child POM. This can help you use unified versions for dependencies for child modules, without specifying the version in each child module.

A parent project (Pro-par) defines a dependency under the dependencyManagement:

<pre><code>
&lt;dependencyManagement&gt;
  &lt;dependencies&gt;
    &lt;dependency&gt;
      &lt;groupId&gt;junit&lt;/groupId&gt;
      &lt;artifactId&gt;junit&lt;/artifactId&gt;
      &lt;version&gt;3.8&lt;/version&gt;
    &lt;/dependency&gt;
 &lt;/dependencies&gt;
&lt;/dependencyManagement&gt;
</code></pre>
  
Then in the child of Pro-par, I can use the junit:

<pre><code>
&lt;dependencies&gt;
  &lt;dependency&gt;
    &lt;groupId&gt;junit&lt;/groupId&gt;
    &lt;artifactId&gt;junit&lt;/artifactId&gt;
  &lt;/dependency&gt;
&lt;/dependencies&gt;
</code></pre>

There's still one thing that is not highlighted enough, in my opinion, and that is unwanted inheritance.
Here's an incremental example:

I declare in my parent pom:

<pre><code>
&lt;dependencies&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;com.google.guava&lt;/groupId&gt;
            &lt;artifactId&gt;guava&lt;/artifactId&gt;
            &lt;version&gt;19.0&lt;/version&gt;
        &lt;/dependency&gt;
&lt;/dependencies&gt;
</code></pre>
boom! I have it in my Child A, Child B and Child C modules:

* Implicilty inherited by child poms
* A single place to manage
* No need to redeclare anything in child poms
* I can still redelcare and override to version 18.0 in a Child B if I want to.
But what if I end up not needing guava in Child C, and neither in the future Child D and Child E modules?

They will still inherit it and this is undesired! This is just like Java God Object code smell, where you inherit some useful bits from a class, and a tonn of unwanted stuff as well.

This is where &lt;dependencyManagement&gt; comes into play. When you add this to your parent pom, all of your child modules STOP seeing it. And thus you are forced to go into each individual module that DOES need it and declare it again (Child A and Child B, without the version though).

And, obviously, you don't do it for Child C, and thus your module remains lean.

#### Reference:  https://stackoverflow.com/questions/2619598/differences-between-dependencymanagement-and-dependencies-in-maven
