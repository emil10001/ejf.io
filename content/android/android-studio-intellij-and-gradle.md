{
    "slug": "android-studio-intellij-and-gradle",
    "date": "2013-08-25T02:18:03.000Z",
    "tags": [
        "android",
        "android development",
        "gradle",
        "intellij",
        "android studio"
    ],
    "title": "Android Studio, Intellij and Gradle",
    "publishdate": "2013-08-25T02:18:03.000Z"
}Android Studio, Intellij and Gradle
===================================




<p>I know that I had written a month or so ago about using vim a bit more heavily instead of an IDE. That was kind of silly. While I did use vim quite a bit for doing things in JavaScript, it just can&rsquo;t compete with a proper IDE. Long story short, I switched over to Intellij at work for doing some stuff with AngularJS, and finally got over the hump, and really liked it. So, I figured that it was finally time to use Intellij for Android stuff as well, especially since I want to get a couple new projects started to try out some of the new stuff that&rsquo;s out there for Android. Before I forget, here&rsquo;s the Google I/O talk on the new build system:</p>

<iframe width="560" height="315" src="//www.youtube.com/embed/LCJAgPkpmR0" frameborder="0" allowfullscreen></iframe>

<p>I had spent quite a bit of time writing up a how-to on using Gradle with an Anroid project in Intellij Ultimate 13 EAP, but after sinking a solid 8 hours into trying to get it to work properly, I gave up. While I was able to add a <code>build.gradle</code> file to the project, and run <code>gradle build</code> successfully, I could not get  Intellij to pull dependencies from the Gradle build. I&rsquo;m sure there&rsquo;s a config somewhere that I am missing, but I simply ran out of time, I can&rsquo;t really afford to spend more than 8 hours trying to get my environment set up to start a new project. There were several frustrating things about this. First, we were promised that with the new Android Studio, that the IDE would have deep hooks into the Gradle build system, which would solve the problems that we had seen previously in trying to use Maven for Android projects in Eclipse, for example. Second, the new Android compatibility library is much easier to deal with if you&rsquo;re using Gradle, and without it, you need to jump through a bunch of extra hoops.</p>

<p>The trouble that I was having was that I had based my efforts on a faulty assumption - that Intellij Ultimate would be very similar in handling Android project creation to Android Studio, and should be able to handle the Gradle stuff in the same way as well. This turns out to have been an incorrect assumption. Android project creation is not the same between the two IDEs, and the key difference is that in Android Studio, when you run through the new project wizard, you end up with a Gradle backed project, that&rsquo;s actually configured correctly to leverage Gradle as expected. If you create a new Android project with the wizard in Intellij, you end up with a project that is near impossible to set up Gradle IDE integration for.</p>

<p>I had run through a bunch of different ways of starting a new Android project, or importing one to Intellij, and was never able to get the Gradle integration working. I won&rsquo;t go as far as to say that it is impossible, but again, I spent about 8 hours hammering away at it and wasn&rsquo;t able to get it all the way there. I only managed to get lots of different states of semi-working, but without that all-important IDE integration, which meant that I probably could have used Gradle for building, but not for resolving dependencies.</p>

<p>I added a comment to <a href="http://youtrack.jetbrains.com/issue/IDEA-106313" target="_blank">this bug report</a> for Intellij, asking for better Gradle support for Android projects.</p>

<h2>Resulting project</h2>

<p>Here are a few notes that I made about the Gradle build files and file structure that Android Studio generates.</p>

<p>Notice how the file layout has a parent directory, and then <code>src/main/</code> before you start seeing your project&rsquo;s code. Then, the packages are under the <code>java</code> dir. There are also two <code>build.gradle</code> files.</p>

<script src="https://gist.github.com/emil10001/6315075.js?file=structure_diagram"></script><p>This is the <code>build.gradle</code> that belongs to the parent. We want to use the Gradle warpper, so we&rsquo;ll initialize that here:</p>

<script src="https://gist.github.com/emil10001/6315075.js?file=parent_build.gradle"></script><p>You can run <code>gradle wrapper</code> from the parent directory to generate the wrapper script and supporting files.</p>

<p>Here&rsquo;s the <code>settings.gradle</code> that belongs to the parent, all it does is include your project:</p>

<script src="https://gist.github.com/emil10001/6315075.js?file=settings.gradle"></script><p>The parent&rsquo;s Gradle files would look a bit different if we had multiple projects being pulled in and compiled. For example, if you were including the Facebook SDK or ActionBarSherlock, you&rsquo;d have to specify those projects in <code>settings.gradle</code>.</p>

<p>Now, here&rsquo;s the project&rsquo;s <code>build.gradle</code>:</p>

<script src="https://gist.github.com/emil10001/6315075.js?file=build.gradle"></script><p>Notice that I&rsquo;m including the appcompat and support libraries as dependencies. I made sure that Gradle was configured to use the wrapper and that auto-import was enabled. Auto-import might be an annoyance if you touch your build files often, but for me, I wanted any changes to <code>build.gradle</code> to be reflected immediately in my project. E.g., if I were to add a dependent library, I&rsquo;d want to be able to use it immediately without needing to manually kick off a Gradle command.</p>

<p>In Android Studio, this setup gets me up and running pretty quickly, and I was able to start importing things into my code immediately from those support libraries without doing much else. That&rsquo;s really the beauty of this whole effort, when you&rsquo;re using Android Studio, the difficult setup stuff is just kind of handled for you, and you can actually use a complex dependency management and build tool without much headache.</p>
