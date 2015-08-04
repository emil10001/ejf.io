{
    "slug": "ide-imports-part-6-java-mirror-with-android",
    "date": "2014-04-28T16:07:00.000Z",
    "tags": [
        "Mirror API",
        "java",
        "Google Glass"
    ],
    "title": "IDE Imports Part 6 - Java Mirror with Android Studio",
    "publishdate": "2014-04-28T16:07:00.000Z"
}IDE Imports Part 6 - Java Mirror with Android Studio
====================================================




<p>This is the sixth of a series of posts discussing how to get Google Glass Mirror (Java) and GDK projects set up in various IDEs.</p>

<p>One of the most common questions that I get from people during workshops is how to get set up either the Mirror quick start, or the GDK project into Eclipse, Android Studio or IntelliJ.</p>

<p>This post will cover importing the Java Mirror Quick Start project into Android Studio.</p>

<iframe width="420" height="315" src="//www.youtube.com/embed/8gTcBGeFvwo" frameborder="0" allowfullscreen></iframe>

<h3>0. Clone from GitHub</h3>

<p><a href="http://www.recursiverobot.com/post/84074011973/ide-imports-part-1-git-clone" target="_blank">See previous post.</a></p>

<h3>1. Import Project</h3>

<p>In Android Studio, select <code>Import Project</code>.</p>

<p><img src="https://31.media.tumblr.com/efe6226c1ae2b81a525b5dc930cbbb21/tumblr_inline_n4r0pyolyh1sq0x3a.png" alt=""/></p>

<h3>2. Import project from existing model</h3>

<p>Select Maven</p>

<p><img src="https://31.media.tumblr.com/1105117ea3d8252edc01d0fb30eca105/tumblr_inline_n4r0s2fLy21sq0x3a.png" alt=""/></p>

<h3>3. Hit &lsquo;Next&rsquo; a few times</h3>

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
