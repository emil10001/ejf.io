{
    "slug": "lowfreqlivecard-example-of-a-low-frequency",
    "date": "2014-05-19T15:53:00.000Z",
    "tags": [
        "Google Glass",
        "GDK",
        "gdk",
        "livecard",
        "low frequency livecard",
        "live card"
    ],
    "title": "LowFreqLiveCard - Example of a low frequency LiveCard using GDK for Google Glass",
    "publishdate": "2014-05-19T15:53:00.000Z"
}LowFreqLiveCard - Example of a low frequency LiveCard using GDK for Google Glass
================================================================================




<p><img src="https://31.media.tumblr.com/6b6db9efd589a4bfd21f85687cb9c305/tumblr_inline_n5tvt6ALjp1sq0x3a.png" alt=""/></p>

<p>I wrote an example of a <a href="https://github.com/emil10001/LowFreqLiveCard" target="_blank">low frequency LiveCard</a> using GDK for Google Glass. The master branch uses Gradle, the <a href="https://github.com/emil10001/LowFreqLiveCard/tree/legacy/" target="_blank">legacy branch</a> is able to be imported into Eclipse.</p>

<p>This code is heavily based on the <a href="https://developers.google.com/glass/develop/gdk/live-cards" target="_blank">Live Card documentation</a> on the GDK section of the Glass developer docs. The reason that I wanted to do this was that I wanted something that required as little code as possible to run.</p>

<h3>Code</h3>

<p>Here&rsquo;s the entire source for the <code>Service</code> that creates and publishes the LiveCard. It&rsquo;s very similar to what you see in the documentation.</p>

<script src="https://gist.github.com/emil10001/5bb83953f17b52b60452.js"></script><h3>Notes</h3>

<p>A couple of notes, it uses a custom voice command, launch with:</p>

<pre><code>'Ok Glass, start my awesome app'
</code></pre>

<p>This displays random data, it is not correct, and Glass does not have a built in HRM that I&rsquo;m aware of. It will display a new value every 3 seconds, and will stop updating after 10 values have been generated.</p>

<h3>Importing</h3>

<p><a href="http://www.recursiverobot.com/post/84134813599/ide-imports-part-9-gdk-with-eclipse" target="_blank">Instructions for importing legacy version into Eclipse.</a></p>

<p><a href="http://www.recursiverobot.com/post/84100040486/ide-imports-part-5-gradle-gdk-with-android-studio" target="_blank">Instructions for importing into Android Studio.</a></p>

<p><a href="http://www.recursiverobot.com/post/84081134683/ide-imports-part-3-gradle-gdk-with-intellij" target="_blank">Instructions for importing into IntelliJ.</a> A note on this one, you&rsquo;ll actually import this as a Gradle project, not &lsquo;from existing sources&rsquo;. Everything should be configured correctly to allow it to be imported as a Gradle project.</p>

<h3>Source</h3>

<p>Full source available on <a href="https://github.com/emil10001/LowFreqLiveCard" target="_blank">GitHub</a>.</p>
