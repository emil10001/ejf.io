{
    "slug": "ide-imports-part-8-java-mirror-with-eclipse",
    "date": "2014-04-28T16:25:00.000Z",
    "tags": [
        "Mirror API",
        "java",
        "eclipse",
        "Google Glass"
    ],
    "title": "IDE Imports Part 8 - Java Mirror with Eclipse",
    "publishdate": "2014-04-28T16:25:00.000Z"
}IDE Imports Part 8 - Java Mirror with Eclipse
=============================================




<p>This is the eighth of a series of posts discussing how to get Google Glass Mirror (Java) and GDK projects set up in various IDEs.</p>

<p>One of the most common questions that I get from people during workshops is how to get set up either the Mirror quick start, or the GDK project into Eclipse, Android Studio or IntelliJ.</p>

<p>This post will cover importing the Java Mirror Quick Start project into Eclipse.</p>

<iframe width="560" height="315" src="//www.youtube.com/embed/iXo_d5av_Ak" frameborder="0" allowfullscreen></iframe>

<h3>0. Clone from GitHub</h3>

<p><a href="http://www.recursiverobot.com/post/84074011973/ide-imports-part-1-git-clone" target="_blank">See previous post.</a></p>

<h3>1. Install m2e plugin in Eclipse</h3>

<ul><li>Visit the <a href="http://eclipse.org/m2e/" target="_blank">m2e project site</a></li>
<li>Go to the <a href="http://eclipse.org/m2e/download/" target="_blank">Downloads section</a></li>
<li>Copy the <a href="http://download.eclipse.org/technology/m2e/releases" target="_blank">latest m2e release URL</a></li>
<li>In Eclipse, go to <code>Help</code> -&gt; <code>Install Software</code></li>
<li>Paste the m2e link into the bar, hit &lsquo;enter&rsquo; and give it a name, like m2e</li>
<li>Select the packages that come up when it loads</li>
<li>Install the m2e packages</li>
<li>Accept the license</li>
<li>Restart Eclipse</li>
</ul><h3>2. Import Project</h3>

<p><code>File</code> -&gt; <code>Import Project</code> -&gt; <code>Maven</code> -&gt; <code>Existing Maven Project</code></p>

<h3>3. Hit 'Next&rsquo; a few times</h3>

<p>Then 'Finish&rsquo; and you&rsquo;ll be done with that.</p>

<h3>4. Update the <code>oauth.properties</code> file</h3>

<p><a href="http://www.recursiverobot.com/post/82000172161/google-glass-mirror-api-java-quick-start" target="_blank">I previously made a video with instructions on this.</a> <a href="https://developers.google.com/glass/develop/mirror/quickstart/java" target="_blank">Google has also posted instructions for getting OAuth set up.</a></p>

<h3>5. Fire up a terminal</h3>

<p>Run the following from the root of the project:</p>

<pre><code>$ mvn install
$ mvn jetty:run
</code></pre>

<h3>6. Finished!</h3>

<p>You&rsquo;re all done!</p>
