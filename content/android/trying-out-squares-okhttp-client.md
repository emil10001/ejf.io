{
    "slug": "trying-out-squares-okhttp-client",
    "date": "2013-04-24T16:53:00.000Z",
    "tags": [
        "android",
        "programming",
        "square",
        "okhttp",
        "otto"
    ],
    "title": "Trying out Square's OkHttp client",
    "publishdate": "2013-04-24T16:53:00.000Z"
}Trying out Square's OkHttp client
=================================




<p>This is a simple example of implementing <a href="https://github.com/square/okhttp" target="_blank">Square&rsquo;s OkHttp library</a> in an Android app. Basically, OkHttp is intending to provide a fast HTTP+SPDY java client.</p>

<h3>OkHttp</h3>

<p>Step zero is getting OkHttp either as a jar, or pulling down the project and building a jar with maven. When I built this, the jar was not available, so I needed to build it myself. This was quite simple, all you need to do is make sure you have Maven2 installed, and follow the instructions on OkHttp&rsquo;s github page.</p>

<p>One thing to note is that you need to be building against the parent project, not just the okhttp dir. Sometimes people package up git repos such that there are actually multiple projects in a single repo, and you only need to use one of the dirs in the project. In this case, you need the parent, it will build the child projects for you.</p>

<p>Building:</p>

<pre><code>mvn clean
mvn package -DskipTests
</code></pre>

<p>After you build, the jar will be in okhttp/targets.</p>

<p>This git repo bundles a version of okhttp that I build, but it would be better to pull down a more recent version.</p>

<h3>Otto</h3>

<p>I&rsquo;m also using Otto here. Check out my <a href="http://www.recursiverobot.com/post/48752686831/playing-around-with-otto-on-android" target="_blank">post on Otto</a> for a quick primer.</p>

<h3>Onto the code!</h3>

<p>First up, since we&rsquo;re using Otto, we register with the bus first. Then, the OkHttp logic is placed in an AsyncTask, so we&rsquo;ll kick that off here too. This code lives in an Activity&rsquo;s onCreate.</p>

<script src="https://gist.github.com/emil10001/5453576.js?file=RequestFromMain.java" type="text/javascript"></script><p>Here&rsquo;s the logic for connecting to a site, reading the response into a string, and sending a message out onto the bus with that response string.</p>

<script src="https://gist.github.com/emil10001/5453576.js?file=OkDownload.java" type="text/javascript"></script><p>This is the method that&rsquo;s subscribing to the bus, waiting for that response string.</p>

<script src="https://gist.github.com/emil10001/5453576.js?file=OttoSubscriber.java" type="text/javascript"></script><p><a href="https://github.com/emil10001/OkHttpAndroidExample" target="_blank">Check out the full project here.</a></p>
