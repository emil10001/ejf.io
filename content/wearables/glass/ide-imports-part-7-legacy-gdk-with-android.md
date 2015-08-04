{
    "slug": "ide-imports-part-7-legacy-gdk-with-android",
    "date": "2014-04-28T16:15:28.000Z",
    "tags": [
        "Google Glass",
        "GDK",
        "android studio"
    ],
    "title": "IDE Imports Part 7 - Legacy GDK with Android Studio",
    "publishdate": "2014-04-28T16:15:28.000Z"
}IDE Imports Part 7 - Legacy GDK with Android Studio
===================================================




<p>This is the seventh of a series of posts discussing how to get Google Glass Mirror (Java) and GDK projects set up in various IDEs.</p>

<p>One of the most common questions that I get from people during workshops is how to get set up either the Mirror quick start, or the GDK project into Eclipse, Android Studio or IntelliJ.</p>

<p>This post will cover importing a Legacy GDK project into Android Studio.</p>

<iframe width="420" height="315" src="//www.youtube.com/embed/WC8deG6_pFM" frameborder="0" allowfullscreen></iframe>

<h3>0. Clone from GitHub</h3>

<p><a href="http://www.recursiverobot.com/post/84074011973/ide-imports-part-1-git-clone" target="_blank">See previous post.</a></p>

<h3>1. Import Project</h3>

<p><img src="https://31.media.tumblr.com/efe6226c1ae2b81a525b5dc930cbbb21/tumblr_inline_n4q5ggIrLX1sq0x3a.png" alt=""/></p>

<h3>2. Hit &lsquo;Next&rsquo; a few times</h3>

<p>Until the project is imported.</p>

<p>The project will automatically be converted into a Gradle project.</p>

<h3>3. Add compileSdkVersion to <code>build.gradle</code></h3>

<p>The correct value is:</p>

<pre><code>compileSdkVersion "Google Inc.:Glass Development Kit Preview:19"
</code></pre>

<h3>4. Fix the <code>AndroidManifest.xml</code></h3>

<p>Need to have one Activity that is considered the launch Activity, otherwise IntelliJ (Android Studio) won&rsquo;t let you run.</p>

<script src="https://gist.github.com/emil10001/11358918.js"></script><h3>5. Finished!</h3>

<p>You&rsquo;re all done!</p>
