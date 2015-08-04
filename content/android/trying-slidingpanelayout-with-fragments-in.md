{
    "slug": "trying-slidingpanelayout-with-fragments-in",
    "date": "2013-08-29T02:17:56.000Z",
    "tags": [
        "android",
        "android development"
    ],
    "title": "Trying SlidingPaneLayout with Fragments (in Android)",
    "publishdate": "2013-08-29T02:17:56.000Z"
}Trying SlidingPaneLayout with Fragments (in Android)
====================================================




<p>Building on my <a href="http://www.recursiverobot.com/post/59252588673/android-studio-intellij-and-gradle" target="_blank">last</a> <a href="http://www.recursiverobot.com/post/59267986367/setting-up-the-new-actionbarcompat-in-android-studio" target="_blank">few</a> <a href="http://www.recursiverobot.com/post/59404388046/implementing-the-new-navigation-drawer-in-android" target="_blank">posts</a>, I wanted to try out the new SlidingPaneLayout that&rsquo;s being offered in the support library. For the uninitiated, here&rsquo;s a Google I/O presentation that presents SlidingPaneLayout along with a few other new UI concepts for Android:</p>

<iframe width="560" height="315" src="//www.youtube.com/embed/Jl3-lzlzOJI" frameborder="0" allowfullscreen></iframe>

<p>This was super simple to do, so I&rsquo;m going to go fairly quickly again.</p>

<h2>Instructions</h2>

<h3>0</h3>

<p>My <a href="http://www.recursiverobot.com/post/59252588673/android-studio-intellij-and-gradle" target="_blank">last</a> <a href="http://www.recursiverobot.com/post/59267986367/setting-up-the-new-actionbarcompat-in-android-studio" target="_blank">few</a> <a href="http://www.recursiverobot.com/post/59404388046/implementing-the-new-navigation-drawer-in-android" target="_blank">posts</a>, are prerequisites for this one. So, make sure that you&rsquo;re up to speed on those.</p>

<h3>1</h3>

<p>Add a <code>SlidingPaneLayout</code> section to your activity, it should contain a couple views that you can attach things to later. Since we&rsquo;re attaching fragments, these needed to be some sort of <code>ViewGroup</code>, and <code>FrameLayout</code> implements that.</p>

<script src="https://gist.github.com/emil10001/6373553.js?file=activity_main.xml"></script><h3>2</h3>

<p>In the <code>MainActivity</code>, we want to grab a reference to the SlidingPane, start it as open by default, and attach our content to it. Our content happens to be fragments, but you can attach whatever views you feel like here.</p>

<script src="https://gist.github.com/emil10001/6373553.js?file=MainActivity.java"></script><h3>3</h3>

<p>The fragments are pretty simple, we simply inflate the layout. I also decided to put some text and a color in there.</p>

<script src="https://gist.github.com/emil10001/6373553.js?file=PaneFrag.java"></script><h3>4</h3>

<p>This is the layout file used by the fragments.</p>

<script src="https://gist.github.com/emil10001/6373553.js?file=frags.xml"></script><h2>Conclusion</h2>

<p>This whole thing took less time than it did to install the 0.2.6 upgrade for Android Studio, so if you&rsquo;re thinking about using a <code>SlidingPaneLayout</code>, it&rsquo;s incredibly simple, so just go for it!</p>

<p><a href="https://github.com/emil10001/ActionBarCompatExample/tree/SlidingPane" target="_blank">Github repo here</a>.</p>
