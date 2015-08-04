{
    "slug": "implementing-the-new-navigation-drawer-in-android",
    "date": "2013-08-26T16:39:00.000Z",
    "tags": [
        "android",
        "android development",
        "navigation drawer"
    ],
    "title": "Implementing the new Navigation Drawer in Android",
    "publishdate": "2013-08-26T16:39:00.000Z"
}Implementing the new Navigation Drawer in Android
=================================================




<p>I wanted to try out the new <a href="https://developer.android.com/design/patterns/navigation-drawer.html" target="_blank">Navigation Drawer</a> that&rsquo;s provided in <a href="https://developer.android.com/tools/support-library/index.html" target="_blank">a recent release of the Android Support Library</a>. I was hoping that it would be fairly straight-forward, and it was. Here&rsquo;s <a href="https://developer.android.com/training/implementing-navigation/nav-drawer.html" target="_blank">the official documentation</a>, it&rsquo;s what I&rsquo;m basing this on, including the entirety of my <code>MainActivity.java</code> code, if you&rsquo;re interested in just looking at that, have at it. There&rsquo;s lots of code, and even a sample app to download.</p>

<p><img src="http://files.feigdev.com/rr/navdrawer.png"/></p>

<p>I think that I&rsquo;ll structure this post the same way that I did for my last one, since this is also fairly straight-forward, and builds on my last post.</p>

<h2>How to</h2>

<h3>0</h3>

<p>Have a project set up, for use with AppCompat lib, as I <a href="http://www.recursiverobot.com/post/59267986367/setting-up-the-new-actionbarcompat-in-android-studio" target="_blank">showed in my previous blog post</a>.</p>

<h3>1</h3>

<p>First up, modify your layout to be a drawer layout, and give yourself two children to work with:</p>

<script src="https://gist.github.com/emil10001/6343343.js?file=activity_main.xml"></script><h3>2</h3>

<p>If you&rsquo;re using this code, you&rsquo;ll need some layout to fill in the <code>ListView</code> that appears in the drawer:</p>

<script src="https://gist.github.com/emil10001/6343343.js?file=drawer_list_item.xml"></script><h3>3</h3>

<p>We need a couple of strings to describe the open/close actions, so add these to your <code>strings.xml</code> file:</p>

<script src="https://gist.github.com/emil10001/6343343.js?file=strings.xml"></script><h3>4</h3>

<p>You&rsquo;ll need one of these in your <code>drawables-hdpi</code> directory:</p>

<p><img src="http://files.feigdev.com/rr/ic_drawer.png"/></p>

<p>Really, the proper way to do this is to create a few assets at the appropriate scales, and drop them into each of the drawables directories so that they all look right. <a href="https://developer.android.com/training/implementing-navigation/nav-drawer.html" target="_blank">Official assets can be downloaded here</a>.</p>

<p>I&rsquo;ll also note that the asset may not be in my github repo yet for some reason. Android Studio didn&rsquo;t pick it up as a new file, and while I&rsquo;ll get it pushed up shortly, if you download this before I fix it, you may be missing that asset.</p>

<h3>5</h3>

<p>Now, we can use this in the code. This basically enables the icon in the <code>ActionBar</code> to be used to toggle the drawer, along with swiping from the left edge. It also adds a simple <code>ListView</code> within the drawer, and allows user to click those items to toast the name of the app, and change the title in the <code>ActionBar</code>.</p>

<script src="https://gist.github.com/emil10001/6343343.js?file=MainActivity.java"></script><p>I&rsquo;m not going to give a detailed discussion, if you aren&rsquo;t sure of what&rsquo;s going on, <a href="https://developer.android.com/training/implementing-navigation/nav-drawer.html" target="_blank">please read the official documentation</a>, where most of this code comes from.</p>

<h3>Done!</h3>

<p>Here&rsquo;s the final product:</p>

<p><img src="http://files.feigdev.com/rr/navdrawer_device.png"/></p>

<p>Pretty, isn&rsquo;t it? <a href="https://github.com/emil10001/ActionBarCompatExample/tree/NavDrawer" target="_blank">Here&rsquo;s the repo, on the NavDrawer branch</a></p>
