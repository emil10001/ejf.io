{
    "slug": "an-android-app-to-slow-your-test-device-down",
    "date": "2013-11-26T16:40:00.000Z",
    "tags": [
        "android",
        "development",
        "programming",
        "android development",
        "api"
    ],
    "title": "An Android app to slow your test device down.",
    "publishdate": "2013-11-26T16:40:00.000Z"
}An Android app to slow your test device down.
=============================================




<p>I&rsquo;ve got a bunch of Android devices at my desk, most of which are several years old. I need them for testing the apps that I work on, both to make sure that they work consistently across OEMs, but also to see how the app performs on older devices. The problem that I kept running into was that the old devices would feel faster than my newer phone, simply because there wasn&rsquo;t as much running on them.</p>

<h2>The Solution</h2>

<p>Add some load to those test devices! I decided to create <a href="https://github.com/emil10001/LoadTester" target="_blank">this app to make the phone do some sort of constant and predictable work</a> in the background. It can do three things, load up the CPU, eat up about 130-140MB of memory (limited by the system here), and make network requests.</p>

<p><img src="https://s3.amazonaws.com/ejf3-public/hosted_files/2013-11-26+16.14.38.png" alt="Screenshot"/></p>

<p>I also tried to architect and design the thing in a sane way, hopefully it&rsquo;s not too difficult to understand.</p>

<h2>API</h2>

<p>In addition to launching and configuring manually, there is an API that allows you to broadcast an intent, with all the configuration options that you get in the UI! Here&rsquo;s how you would launch it:</p>

<pre><code>Intent launchLoadTest = new Intent();
launchLoadTest.setAction("com.feigdev.loadtester.api");
// Time in miliseconds
long keepAlive = 20 * 1000;
launchLoadTest.putExtra("KEEP_ALIVE", keepAlive);
launchLoadTest.putExtra("CPU_ENABLED", true);
launchLoadTest.putExtra("RAM_ENABLED", true);
launchLoadTest.putExtra("NET_ENABLED", true);
// Valid modes are "LOW", "MEDIUM" and "HIGH"
launchLoadTest.putExtra("MODE", LOW");
sendBroadcast(launchLoadTest);
</code></pre>

<p>And, when you&rsquo;re done, you can kill it too:</p>

<pre><code>Intent launchLoadTest = new Intent();
launchLoadTest.setAction("com.feigdev.loadtester.api");
launchLoadTest.putExtra("KILL_NOW", "KILL_NOW");
sendBroadcast(launchLoadTest);
</code></pre>

<p><a href="https://github.com/emil10001/LoadTestLauncher" target="_blank">Here is a project that launches and kills the LoadTester.</a></p>

<h2>Notes</h2>

<p>The configurations for load might need to be tweaked. It&rsquo;s all just guess work at this point, and while I was able to put enough load on my Nexus 5 to freeze it up completely, I haven&rsquo;t figured out what the best parameters are for a consistent load on each setting. Feel free to make pull requests with different values.</p>

<p>As far as walking through the code, I might break that out into several posts that look at the different components. I&rsquo;m thinking that there are at least a few good topics in there that would be worth a short discussion.</p>

<p>If you&rsquo;re looking for an apk, <a href="https://s3.amazonaws.com/ejf3-public/hosted_files/LoadTester-debug-unaligned.apk" target="_blank">here&rsquo;s one to download</a>.</p>

<p><a href="https://github.com/emil10001/LoadTester" target="_blank">Source on GitHub</a></p>
