{
    "slug": "netflix-and-audible-on-glass",
    "date": "2014-01-29T18:39:00.000Z",
    "tags": [
        "glass",
        "googleglass",
        "google glass"
    ],
    "title": "Netflix (and Audible) on Glass!",
    "publishdate": "2014-01-29T18:39:00.000Z"
}Netflix (and Audible) on Glass!
===============================




<p><img src="https://31.media.tumblr.com/70dee588d5e21e6fa05257dd8e405b9b/tumblr_inline_n05c91ionZ1sq0x3a.jpg" alt=""/></p>

<p>Did you know that you could install regular Android apps on Glass? What&rsquo;s more, some of them don&rsquo;t crash immediately, and actually kind of work! This one&rsquo;s pretty simple, here&rsquo;s what you&rsquo;re going to need:</p>

<ul><li>Google Glass</li>
<li>Bluetooth keyboard and mouse</li>
<li><a href="https://coderwall.com/p/uzviqa" target="_blank">Settings.apk</a></li>
<li>Ability to get apks for whatever apps you want to install</li>
<li>Android SDK installed</li>
<li>Ability to build and install Android apps from source</li>
</ul><p>I&rsquo;m not going to post links to apps&rsquo; apks, because that&rsquo;s not cool. You&rsquo;ll need to figure that bit out yourselves. (Hint, use a rooted phone.)</p>

<p>Start off by putting your device into debug mode, if you haven&rsquo;t already. Next, grab <a href="https://coderwall.com/p/uzviqa" target="_blank">Settings.apk</a> and install it on Glass.</p>

<pre><code>adb install Settings.apk
</code></pre>

<p>After that, clone, compile and install <a href="https://github.com/justindriggers/Glass-Launcher" target="_blank">Glass Launcher from GitHub</a>. If you aren&rsquo;t familiar with how to do this, try the following:</p>

<pre><code>git clone git@github.com:justindriggers/Glass-Launcher.git
cd Glass-Launcher
ant debug
adb install bin/*.apk 
</code></pre>

<blockquote>
  <p>Disclaimer, I don&rsquo;t build from the command-line, so YMMV with the above commands. Also, probably specify the actual apk instead of *.apk, I&rsquo;m just not sure what it&rsquo;s name is going to be once compiled.</p>
</blockquote>

<p>After you have both Glass Launcher and Settings installed, try using Glass Launcher to launch the Settings app. You can use the touchpad to highlight and select different elements. You should be able to navigate into the Bluetooth settings, and pair your keyboard and mouse. Get those set up now.</p>

<p>Download and install an app that you want, like Netflix or Audible. Now you can use the keyboard to log into your account on Netflix or Audible. So far, so good. For Netflix, this is really all you need. If you want to use Audible, you&rsquo;ll need to get the mouse out in order to select the title that you want to download. You&rsquo;ll also need to use the mouse to start playing a title initially, but once you have something in your &lsquo;currently playing&rsquo; section, you can get by with just the touchpad.</p>

<p>All set! Now you can enjoy your books and movies right on Glass!</p>
