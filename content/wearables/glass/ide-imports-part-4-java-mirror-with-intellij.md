{
    "slug": "ide-imports-part-4-java-mirror-with-intellij",
    "date": "2014-04-28T04:42:00.000Z",
    "tags": [
        "Google Glass",
        "Mirror API",
        "java",
        "intellij"
    ],
    "title": "IDE Imports Part 4 - Java Mirror with IntelliJ",
    "publishdate": "2014-04-28T04:42:00.000Z"
}IDE Imports Part 4 - Java Mirror with IntelliJ
==============================================




<p>This is the fourth of a series of posts discussing how to get Google Glass Mirror (Java) and GDK projects set up in various IDEs.</p>

<p>One of the most common questions that I get from people during workshops is how to get set up either the Mirror quick start, or the GDK project into Eclipse, Android Studio or IntelliJ.</p>

<p>This post will cover importing the Java Mirror Quick Start project into IntelliJ.</p>

<iframe width="420" height="315" src="//www.youtube.com/embed/sI1B3SFbEvg" frameborder="0" allowfullscreen></iframe>

<h3>0. Clone from GitHub</h3>

<p><a href="http://www.recursiverobot.com/post/84074011973/ide-imports-part-1-git-clone" target="_blank">See previous post.</a></p>

<h3>1. Import Project</h3>

<p>In IntelliJ, select <code>Import Project</code>.</p>

<p><img src="https://31.media.tumblr.com/72e5c4929a5dc7fc5761f010e191649a/tumblr_inline_n4prz8JBz11sq0x3a.png" alt=""/></p>

<h3>2. Create project from existing sources</h3>

<p>Or you can import it as a Maven project. Either way works</p>

<p><img src="https://31.media.tumblr.com/f0fe967e915f377d5e1a668db280e798/tumblr_inline_n4ps6fxKcF1sq0x3a.png" alt=""/></p>

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
