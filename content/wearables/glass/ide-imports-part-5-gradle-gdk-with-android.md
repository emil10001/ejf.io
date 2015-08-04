{
    "slug": "ide-imports-part-5-gradle-gdk-with-android",
    "date": "2014-04-28T04:56:00.000Z",
    "tags": [
        "android studio",
        "gdk",
        "Google Glass",
        "gradle"
    ],
    "title": "IDE Imports Part 5 - Gradle GDK with Android Studio",
    "publishdate": "2014-04-28T04:56:00.000Z"
}IDE Imports Part 5 - Gradle GDK with Android Studio
===================================================




<p>This is the fifth of a series of posts discussing how to get Google Glass Mirror (Java) and GDK projects set up in various IDEs.</p>

<p>One of the most common questions that I get from people during workshops is how to get set up either the Mirror quick start, or the GDK project into Eclipse, Android Studio or IntelliJ.</p>

<p>This post will cover importing a Gradle GDK project into Android Studio.</p>

<iframe width="420" height="315" src="//www.youtube.com/embed/tO7iUB6QF1I" frameborder="0" allowfullscreen></iframe>

<h3>0. Clone from GitHub</h3>

<p><a href="http://www.recursiverobot.com/post/84074011973/ide-imports-part-1-git-clone" target="_blank">See previous post.</a></p>

<h3>1. Create New Project</h3>

<p><img src="https://31.media.tumblr.com/efe6226c1ae2b81a525b5dc930cbbb21/tumblr_inline_n4q5ggIrLX1sq0x3a.png" alt=""/></p>

<h3>2. Set Android SDK to use GDK 19</h3>

<p><img src="https://31.media.tumblr.com/091119cedd52d7dfe49f7783e517acd7/tumblr_inline_n4q5inWw0B1sq0x3a.png" alt=""/></p>

<h3>3. Hit &lsquo;Next&rsquo; a few times</h3>

<p>Until the project is imported.</p>

<h3>4. Import Module</h3>

<p>Import the GDK project as a module, and it will automatically be placed in the correct spot, and the Gradle configuration handled correctly.</p>

<h3>5. Delete the dummy project</h3>

<p>No reason to keep it around now.</p>

<h3>6. Add compileSdkVersion to <code>build.gradle</code></h3>

<p>The correct value is:</p>

<pre><code>compileSdkVersion "Google Inc.:Glass Development Kit Preview:19"
</code></pre>

<h3>7. Fix the <code>AndroidManifest.xml</code></h3>

<p>Need to have one Activity that is considered the launch Activity, otherwise IntelliJ won&rsquo;t let you run.</p>

<script src="https://gist.github.com/emil10001/11358918.js"></script><h3>8. Finished!</h3>

<p>You&rsquo;re all done!</p>
