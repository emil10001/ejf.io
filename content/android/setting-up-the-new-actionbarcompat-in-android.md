{
    "slug": "setting-up-the-new-actionbarcompat-in-android",
    "date": "2013-08-25T05:27:00.000Z",
    "tags": [
        "android",
        "android development",
        "gradle"
    ],
    "title": "Setting up the new ActionBarCompat in Android Studio",
    "publishdate": "2013-08-25T05:27:00.000Z"
}Setting up the new ActionBarCompat in Android Studio
====================================================




<p>Continuing from my previous post, I&rsquo;m toying around with a couple new projects, and I wanted to use some of the new <a href="https://developer.android.com/tools/support-library/index.html" target="_blank">compatibility libraries</a> that Google&rsquo;s offering for Android. Unfortunately, the setup is a bit involved, and the instructions are a bit scattered. I figured that it might be worth collecting them in a quick post. My main source of this info was the following DevBytes video:</p>

<iframe width="560" height="315" src="//www.youtube.com/embed/6TGgYqfJnyc" frameborder="0" allowfullscreen></iframe>

<h2>How to</h2>

<h3>1</h3>

<p>Create a new project in Android Studio. I&rsquo;ll skip the long explanation here, but if you have trouble with it, <a href="http://www.recursiverobot.com/post/59252588673/android-studio-intellij-and-gradle" target="_blank">check out my previous post</a>, it should get you going.</p>

<p><strong>EDIT</strong>: Don&rsquo;t forget to install the <a href="https://developer.android.com/tools/support-library/setup.html" target="_blank">Support Libraries</a> in the Android SDK manager.</p>

<h3>2</h3>

<p>The Gradle dependencies that we need are the same as was shown in my previous post, but here&rsquo;s the <code>build.gradle</code> file for this project, just the same:</p>

<p>Add dependency in <code>build.gradle</code></p>

<script src="https://gist.github.com/emil10001/6332117.js?file=build.gradle"></script><h3>3</h3>

<p>Adding a theme to the Activity in the manifest is really simple:</p>

<script src="https://gist.github.com/emil10001/6332117.js?file=activity_in_mainfest.xml"></script><h3>4</h3>

<p>The theme needs to exist, and the parent must be one of the AppCompat provided themes.</p>

<script src="https://gist.github.com/emil10001/6332117.js?file=styles.xml"></script><pre><code>Note: Any custom theme elements need to be defined twice, once for the compatibility version in the default namespace, one for the higher API levels in the android namespace.
</code></pre>

<h3>5</h3>

<p>Go to the Activity, and change it to extend <code>ActionBarActivity</code> instead of <code>Activity</code>.</p>

<script src="https://gist.github.com/emil10001/6332117.js?file=Activity.java"></script><h3>6</h3>

<p>Add a custom namespace to the <code>menu.xml</code>. Menu items need to be added using the support library, using the custom namespace.</p>

<script src="https://gist.github.com/emil10001/6332117.js?file=menu.xml"></script><p>And that&rsquo;s it! You should be all set, be sure to give it a shot on both Froyo or Gingerbread devices as well as ICS and above. Here&rsquo;s the <a href="https://github.com/emil10001/ActionBarCompatExample" target="_blank">full repo on github</a>.</p>
