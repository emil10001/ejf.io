{
    "slug": "ide-imports-part-2-legacy-gdk-with-intellij",
    "date": "2014-04-28T00:59:00.000Z",
    "tags": [
        "Google Glass",
        "GDK",
        "intellij"
    ],
    "title": "IDE Imports Part 2 - legacy GDK with IntelliJ",
    "publishdate": "2014-04-28T00:59:00.000Z"
}IDE Imports Part 2 - legacy GDK with IntelliJ
=============================================




<p>This is the second of a series of posts discussing how to get Google Glass Mirror (Java) and GDK projects set up in various IDEs.</p>

<p>One of the most common questions that I get from people during workshops is how to get set up either the Mirror quick start, or the GDK project into Eclipse, Android Studio or IntelliJ.</p>

<p>This post will cover importing a legacy style GDK project into IntelliJ.</p>

<iframe width="420" height="315" src="//www.youtube.com/embed/HCDkuwL4nKw" frameborder="0" allowfullscreen></iframe>

<h3>0. Clone from GitHub</h3>

<p><a href="http://www.recursiverobot.com/post/84074011973/ide-imports-part-1-git-clone" target="_blank">See previous post.</a></p>

<h3>1. Import Project</h3>

<p>In IntelliJ, select <code>Import Project</code>.</p>

<p><img src="https://31.media.tumblr.com/72e5c4929a5dc7fc5761f010e191649a/tumblr_inline_n4prz8JBz11sq0x3a.png" alt=""/></p>

<h3>2. Create project from existing sources</h3>

<p>Do not import it as an Eclipse, Gradle or Maven project.</p>

<p><img src="https://31.media.tumblr.com/f0fe967e915f377d5e1a668db280e798/tumblr_inline_n4ps6fxKcF1sq0x3a.png" alt=""/></p>

<h3>3. Hit &lsquo;Next&rsquo; a few times</h3>

<p>Until you get to the Project SDK screen.</p>

<h3>4. Select GDK 19</h3>

<p><img src="https://31.media.tumblr.com/f147c71a20e83ed0d69efeb07f88c86e/tumblr_inline_n4pst7Iemn1sq0x3a.png" alt=""/></p>

<h3>5. Hit 'Next&rsquo; a few more times</h3>

<p>Then 'Finish&rsquo; to create the project.</p>

<h3>6. Add a Run Configuration</h3>

<p><img src="https://31.media.tumblr.com/801168a3edd4c01734352d713cbc912a/tumblr_inline_n4pt2fWfwZ1sq0x3a.png" alt=""/></p>

<h3>7. Fix the <code>AndroidManifest.xml</code></h3>

<p>Need to have one Activity that is considered the launch Activity, otherwise IntelliJ won&rsquo;t let you run.</p>

<script src="https://gist.github.com/emil10001/11358918.js"></script><h3>8. Finished!</h3>

<p>You&rsquo;re all done!</p>
