{
    "slug": "ide-imports-part-3-gradle-gdk-with-intellij",
    "date": "2014-04-28T01:27:48.000Z",
    "tags": [
        "GDK",
        "gradle",
        "Google Glass",
        "java",
        "intellij"
    ],
    "title": "IDE Imports Part 3 - Gradle GDK with IntelliJ",
    "publishdate": "2014-04-28T01:27:48.000Z"
}IDE Imports Part 3 - Gradle GDK with IntelliJ
=============================================




<p>This is the third of a series of posts discussing how to get Google Glass Mirror (Java) and GDK projects set up in various IDEs.</p>

<p>One of the most common questions that I get from people during workshops is how to get set up either the Mirror quick start, or the GDK project into Eclipse, Android Studio or IntelliJ.</p>

<p>This post will cover importing a Gradle GDK project into IntelliJ.</p>

<iframe width="420" height="315" src="//www.youtube.com/embed/eI3RPFexI1o" frameborder="0" allowfullscreen></iframe>

<h3>0. Clone from GitHub</h3>

<p><a href="http://www.recursiverobot.com/post/84074011973/ide-imports-part-1-git-clone" target="_blank">See previous post.</a></p>

<h3>1. Create New Project</h3>

<p>In IntelliJ, select Create New Project.</p>

<p><img src="https://31.media.tumblr.com/72e5c4929a5dc7fc5761f010e191649a/tumblr_inline_n4ptlyvXXd1sq0x3a.png" alt=""/></p>

<h3>2. Gradle Android Module</h3>

<p><img src="https://31.media.tumblr.com/db7b515c7d0d0abd368db61a99e5ff33/tumblr_inline_n4ptq5zc3R1sq0x3a.png" alt=""/></p>

<h3>3. Set SDK targets</h3>

<p>Set all of them to 19, and the &lsquo;Compile with&rsquo; to GDK Preview 19.</p>

<p><img src="https://31.media.tumblr.com/9cccbb7558287c89d12457b6eb7c74a9/tumblr_inline_n4ptucoB2b1sq0x3a.png" alt=""/></p>

<h3>4. File -&gt; Import Module</h3>

<p>Now, we&rsquo;re going to import the project that we want to work with. Make sure to select the project here.</p>

<h3>5. Create project from existing sources</h3>

<p>Do not import it as an Eclipse, Gradle or Maven project.</p>

<p><img src="https://31.media.tumblr.com/f0fe967e915f377d5e1a668db280e798/tumblr_inline_n4pu08FA791sq0x3a.png" alt=""/></p>

<h3>6. Hit ‘Next’ a few times</h3>

<p>Until it imports.</p>

<h3>7. Fix project root</h3>

<ul><li>Open the 'Module Settings&rsquo; for the imported module</li>
<li>Go to the 'Modules&rsquo; section</li>
<li>Remove the current 'Content Root&rsquo;</li>
<li>Add the correct 'Content Root&rsquo;, which is the base directory of the module</li>
<li>Make sure sources and resources are annotated correctly</li>
<li>Hit 'OK&rsquo;</li>
</ul><p><img src="https://31.media.tumblr.com/d706e0c04cbbdacc8ef8819b3adc47c1/tumblr_inline_n4pu9zE4lG1sq0x3a.png" alt=""/></p>

<h3>8. Move imported module into root of the previously created project</h3>

<p>This should fix the <code>build.gradle</code> file, you should see it go from red to white (or black). Now, you can also delete the dummy module that&rsquo;s in there already.</p>

<h3>9. Add compileSdkVersion to build.gradle</h3>

<p>The correct value is:</p>

<pre><code>compileSdkVersion "Google Inc.:Glass Development Kit Preview:19"
</code></pre>

<h3>10. Select GDK 19</h3>

<p>Go back into the Module Settings, then select our imported module, and the 'Dependencies&rsquo; tab. From the drop-down, pick GDK 19.</p>

<p><img src="https://31.media.tumblr.com/e385b77f4a0b58f5606bf124ec5fd0fc/tumblr_inline_n4pune8Lf01sq0x3a.png" alt=""/></p>

<h3>11. Add a Run Configuration</h3>

<p><img src="https://31.media.tumblr.com/801168a3edd4c01734352d713cbc912a/tumblr_inline_n4punxGTe81sq0x3a.png" alt=""/></p>

<h3>12. Fix the AndroidManifest.xml</h3>

<p>Need to have one Activity that is considered the launch Activity, otherwise IntelliJ won’t let you run.</p>

<script src="https://gist.github.com/emil10001/11358918.js"></script><h3>13. Finished!</h3>

<p>You’re all done!</p>
