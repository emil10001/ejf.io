{
    "slug": "ide-imports-part-9-gdk-with-eclipse",
    "date": "2014-04-28T16:37:02.000Z",
    "tags": [
        "Google Glass",
        "GDK",
        "java",
        "eclipse"
    ],
    "title": "IDE Imports Part 9 - GDK with Eclipse",
    "publishdate": "2014-04-28T16:37:02.000Z"
}IDE Imports Part 9 - GDK with Eclipse
=====================================




<p>This is the ninth of a series of posts discussing how to get Google Glass Mirror (Java) and GDK projects set up in various IDEs.</p>

<p>One of the most common questions that I get from people during workshops is how to get set up either the Mirror quick start, or the GDK project into Eclipse, Android Studio or IntelliJ.</p>

<p>This post will cover importing a GDK project into Eclipse.</p>

<iframe width="420" height="315" src="//www.youtube.com/embed/c9YDW0JOGq8" frameborder="0" allowfullscreen></iframe>

<h3>0. Clone from GitHub</h3>

<p><a href="http://www.recursiverobot.com/post/84074011973/ide-imports-part-1-git-clone" target="_blank">See previous post.</a></p>

<h3>1. Import Project</h3>

<p><code>File</code> -&gt; <code>Import Project</code> -&gt; <code>Android</code> -&gt; <code>Existing Android Code Into Workspace</code></p>

<p><img src="https://31.media.tumblr.com/86ba24a6e564caae2b80b42eaa8a2eda/tumblr_inline_n4r1wiVMaL1sq0x3a.png" alt=""/></p>

<h3>2. Select the directory with your project</h3>

<p>Make sure that all your source is selected. Then &lsquo;Finish&rsquo; and you&rsquo;ll be done with that.</p>

<h3>3. Edit <code>Project Properties</code></h3>

<p>Right click the project, select <code>Properties</code>.</p>

<h3>4. Set the SDK as GDK 19</h3>

<p>In Project Properties, go to <code>Android</code>, then select the correct SDK.</p>

<p><img src="https://31.media.tumblr.com/68f0556b9bd222404db48a94f299547d/tumblr_inline_n4r22oAWoG1sq0x3a.png" alt=""/></p>

<h3>5. Finished!</h3>

<p>You&rsquo;re all done!</p>
