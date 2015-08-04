{
    "slug": "glassgif-notes-for-a-live-coding-session-using",
    "date": "2014-03-13T05:17:00.000Z",
    "tags": [
        "google glass",
        "glass",
        "googleglass",
        "gdk"
    ],
    "title": "GlassGif - Notes for a live coding session using the GDK",
    "publishdate": "2014-03-13T05:17:00.000Z"
}GlassGif - Notes for a live coding session using the GDK
========================================================




<p>This post is intended to serve as a guide to building a GDK app, and I will be using it as <a href="http://www.meetup.com/SF-ANDROID-LIVE-CODE/events/165572102/" target="_blank">I build one live on stage</a>. The code that will be discussed here is mostly UI, building the Activity and Fragments, as opposed to dealing with lower level stuff, or actually building GIFs. If you&rsquo;re interested, the code for building GIFs is included in the library project that I&rsquo;ll be using throughout, <a href="https://github.com/emil10001/ReusableAndroidUtils" target="_blank">ReusableAndroidUtils</a>. The finished, complete, working code for <a href="https://github.com/emil10001/GlassGifCamera" target="_blank">this project can be found here</a>.</p>

<h2>Video</h2>

<iframe width="560" height="315" src="//www.youtube.com/embed/lE9tyTWWb4A" frameborder="0" allowfullscreen></iframe>

<h2>Project setup (GDK, library projects)</h2>

<p>Create a new project.</p>

<p><img src="https://31.media.tumblr.com/58c9fb13cf33919e5ca2ef8c1e2d2e4c/tumblr_inline_n26yh4VBr11sq0x3a.png" alt=""/></p>

<p>Select <code>Gradle: Android Module</code>, then give your project a name.</p>

<p><img src="https://31.media.tumblr.com/afa5b0a6ec15fdada3c0b1c8c0c76dbf/tumblr_inline_n26yhdO1CL1sq0x3a.png" alt=""/></p>

<p>Create a package, then set the API levels to 15, and the &lsquo;compile with&rsquo; section to GDK, API 15.</p>

<p><img src="https://31.media.tumblr.com/507c1a2f41236b9f5c575e52422957da/tumblr_inline_n26yhkFg2a1sq0x3a.png" alt=""/></p>

<p>Let&rsquo;s do a <code>Blank Activity</code>.</p>

<p><img src="https://31.media.tumblr.com/5a8dd1b595ae90cd175dcd86afc3210a/tumblr_inline_n26yhsca3G1sq0x3a.png" alt=""/></p>

<p>If gradle is yelling at you about <code>local.properties</code>, create <code>local.properties</code> file with the following:</p>

<pre><code>sdk.dir=/Applications/IntelliJ IDEA 13.app/sdk/ 
</code></pre>

<p>Or, whatever location your Android SDK is installed at, then refresh Gradle.</p>

<p>You might need to re-select GDK. If your project is broken and not compiling. Highlight the module, and hit 'F4&rsquo; in Intellij/Android Studio.</p>

<p><img src="https://31.media.tumblr.com/938df0a81c9bfd77e711d7fe97f8580d/tumblr_inline_n26yhzcOzk1sq0x3a.png" alt=""/></p>

<p>Now the project should be building.</p>

<h2>Hello world &amp; run</h2>

<p>Let&rsquo;s start out by just running 'hello world&rsquo;. I&rsquo;m adding this as a module, instead of creating a new project. As such, the manifest is not set up correctly, and needs to be modified:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=AndroidManifest_1.xml"></script><p>Make sure it runs.</p>

<h2>Fullscreen &amp; run</h2>

<p>Update the manifest to change the app&rsquo;s theme to be <code>NoActionBar.Fullscreen</code>:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=AndroidManifest_2.xml"></script><p>Make sure it runs.</p>

<h2>Voice launch &amp; demo</h2>

<p>Turn the activity into an immersion:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=AndroidManifest_3.xml"></script><p><a href="http://blog.glassdiary.com/post/70365887264/gdk-voice-trigger-sample-app" target="_blank">I found this great blog post on adding voice actions.</a> Add the voice_launch.xml:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=voice_launch.xml"></script><p>And the string to strings.xml:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=strings_1.xml"></script><p>Run, and test saying <code>OK Glass, launch the app</code>.</p>

<h2>Image capture &amp; demo</h2>

<p>Refactor structure to make the fragment its own file, <code>ImageGrabFrag</code>. Now, we are going to add some code to deal with the camera. <a href="https://developer.android.com/reference/android/hardware/Camera.html" target="_blank">The Android documentation on the Camera APIs is quite useful.</a>
<a href="https://stackoverflow.com/questions/21723557/java-lang-runtimeexception-takepicture-failed" target="_blank">Here are</a> a <a href="https://stackoverflow.com/questions/21657919/java-lang-runtime-exception-take-picture-failed" target="_blank">couple of</a> StackOverflow discussions that <a href="https://stackoverflow.com/questions/12705226/android-fail-to-connect-to-camera-service-at-camera-open" target="_blank">might be helpful</a> for common Camera problems.</p>

<p>Next, we need to add a <code>SurfaceView</code> to main fragment.</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=fragment_main.xml"></script><p>Now, we need to add a dependency on my library project, <a href="https://github.com/emil10001/ReusableAndroidUtils" target="_blank">ReusableAndroidUtils</a>, which contains a lot of helper code. To do this, clone the repo and import it as a module into your project. After you&rsquo;ve imported it, update your <code>build.gradle</code> file to reflect the addition:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=build_1.gradle"></script><p>Next, I&rsquo;m going to pull in a bunch of code for <code>ImageGrabFrag</code>. Here&rsquo;s what we have at this stage for that file:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=ImageGrabFrag_1.java"></script><p>Finally, let&rsquo;s update the manifest with the required permissions.</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=AndroidManifest_final.xml"></script><p>Copy that in, make sure it all compiles, and try running. At this point, you should get a garbled preview that looks all funky:</p>

<p><img src="https://31.media.tumblr.com/cca6d77e7f0a6e45e00d9a87c2cce103/tumblr_inline_n2b47ba9s91sq0x3a.png" alt=""/></p>

<p>What&rsquo;s happening here? Well, Glass requires some special parameters to initialize the camera properly. Take a look at the updated initCamera method:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=initCamera.java"></script><p>Ok, almost there. Now, we just need to capture an image. Take note of the added <code>takePicture()</code> call at the end of <code>initCamera()</code>. When we initialize the camera now, we&rsquo;re going to try taking a picture immediately. Now we are running into an odd issue.</p>

<p>Try launching the app from the IDE, then try launching it with a voice command. When you launch from your IDE or the command line, it should work. If you launch from a voice command, it will crash! There are a couple things that we need to change. First, there&rsquo;s a race condition going on right now. We are currently initializing the camera after the SurfaceHolder has been initialized, which is good, because if that wasn&rsquo;t initialized, the camera would fail to come up. However, when we launch with a voice command, the microphone is locked by the system, listening to our voice command. The Camera needs the mic to be unlocked, because we might be trying to record a video. Thus, we get a crash.</p>

<p>The error, specifically, is the following:</p>

<p><code>Camera﹕ Unknown message type 8192</code></p>

<p>There are several discussions about this issue floating around:</p>

<ul><li><a href="https://stackoverflow.com/questions/20295479/getting-intermittent-errors-when-acquiring-the-google-glass-camera" target="_blank">[1]</a></li>
<li><a href="https://stackoverflow.com/questions/20153535/google-gdk-differences-in-calling-app-with-voice-trigger-or-menu-affecting-came/20154537#20154537" target="_blank">[2]</a></li>
<li><a href="https://code.google.com/p/google-glass-api/issues/detail?id=259" target="_blank">[3]</a></li>
<li><a href="https://code.google.com/p/google-glass-api/issues/detail?id=351" target="_blank">[4]</a></li>
<li><a href="https://github.com/dazza222/GlassCameraSnapshot/blob/master/src/com/darrenvenn/glasscamerasnapshot/GlassSnapshotActivity.java" target="_blank">[5]</a></li>
</ul><p>The thing to do is to delay initializing the camera a bit:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=ImageGrabFrag_changes_1.java"></script><p>Try running again, and it should work this time.</p>

<h2>Multi-Image capture</h2>

<p>This part&rsquo;s simple. We just modify our image captured callback, and add a counter.</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=pictureTaken_updated.java"></script><p>We&rsquo;re also going to add a static list of files to the main activity, to keep track of them. We&rsquo;ll need that later.</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=listOfFiles.java"></script><h2>Combine to gif</h2>

<p>Now we need another fragment. We&rsquo;re going to do a really simple fragment transaction, and we&rsquo;ll add an interface to help keep things clean. Here&rsquo;s the interface:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=GifFlowControl.java"></script><p>We need a reference to it in the <code>ImageGrabFrag</code>:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=onAttach.java"></script><p>Then we need to change what happens when we finish capturing our last photo:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=pictureTaken_startBuild.java"></script><p>Here&rsquo;s the final version of the <code>ImageGrabFrag</code>, before we leave it:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=ImageGrabFrag_final.java"></script><p>Alright, time to add a new fragment. This one needs a layout, so here&rsquo;s that:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=builder2.xml"></script><p>And here&rsquo;s the fragment that uses that layout:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=GifBuilderFrag.java"></script><p>While there is a fair bit of code here, what&rsquo;s going on is pretty simple. We are kicking off an <code>AsyncTask</code> that iterates through the files, builds <code>Bitmaps</code> and adds those bitmaps to an <code>ArrayList</code> of <code>Bitmaps</code>. When that&rsquo;s done, we can move on. <a href="http://stackoverflow.com/a/20277649/974800" target="_blank">Here&rsquo;s a link to a StackOverflow discussion that explains how to build gifs.</a></p>

<p>For any of this to work, we need to actually do the fragment transactions. So, here is the updated <code>Activity</code> code:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=GGMainActivity.java"></script><p>Now we can run, and it will do stuff, but on Glass, it&rsquo;s not going to be a very good experience. For one thing, you won&rsquo;t have any idea what&rsquo;s going on, as the user, while the gif is being built. For that, let&rsquo;s do two things. First, we should keep the screen on. Update the <code>Activity</code>&rsquo;s <code>onCreate</code> method:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=GGMainActivity_screenOn.java"></script><p>Let&rsquo;s also add a bit of eye candy to the fragment. I want to flip through the images as they&rsquo;re being worked on. I also want a <code>ProgressBar</code> to be updated until the builder is finished.</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=GifBuilderFrag_eyeCandy.java"></script><p>Run it again, that&rsquo;s a little better, right?</p>

<h2>Static Card</h2>

<p>First thing that I&rsquo;d like to do is to <a href="https://developers.google.com/glass/develop/gdk/ui/theme-widgets#creating_glass-styled_cards" target="_blank">insert a <code>Card</code> into the timeline with the image</a>. Now, this works, but it&rsquo;s not great. Currently, <code>StaticCard</code>s are very simple. They do not offer much functionality at the moment, as the API is still being fleshed out. Hopefully soon it will be updated. For now though, let&rsquo;s do what we can:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=insertStaticCard.java"></script><p>As a note, if you have not already, make sure that Gradle is set to build with the GDK:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=build_2.gradle"></script><h2>Viewer</h2>

<p>The <code>StaticCard</code> is OK, but not great for a viewer. We made a gif, not a single image. Let&rsquo;s build a viewer. First thing is that we&rsquo;ll add our last <code>Fragment</code> transaction method to the <code>Activity</code>.</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=startDisplay.java"></script><p>The viewer itself is very simple. It&rsquo;s just a <code>WebView</code> that is included in the library project. We don&rsquo;t even need a layout for it!</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=GifDisplayFrag_1.java"></script><p>There&rsquo;s a bit of an issue with not being able to see the whole thing. I didn&rsquo;t dig into figuring out how to fix that issue. There are other ways of getting gifs to display, but I couldn&rsquo;t get them to work in a reasonable amount of time.</p>

<h2>Getting the gif off Glass</h2>

<p>This section almost warrants its own discussion, but basically the issue is this, without writing a bunch of code, and uploading the gif to your own server, there&rsquo;s not really a nice, official way of sharing the gif. Ideally, that <code>StaticCard</code> that we created would support <code>MenuItem</code>s, which would allow us to share out to other Glassware with a <code>Share</code> menu.</p>

<p>Luckily, I found <a href="http://divingintoglass.blogspot.com/2014/01/how-to-upload-files-using-google-drive.html" target="_blank">this great blog post</a> that provided a nice work-around. It describes how to share to Google Drive, and with only a few lines of code. Here&rsquo;s the code:</p>

<script src="https://gist.github.com/emil10001/9456564.js?file=GifDisplayFrag_final.java"></script><p>It&rsquo;s not the sort of thing that I&rsquo;d feel comfortable releasing, however, it is enough to finish up here.</p>

<h2>Wrap-up</h2>

<p><img src="https://lh6.googleusercontent.com/-xbQNCUfPceQ/Ux3uqmCXydI/AAAAAAABlfs/XJhlfno8dUU/w960-h720-no/1394397750671.gif" alt=""/></p>

<p><a href="https://github.com/emil10001/GlassGifCamera" target="_blank">GlassGifCamera code on GitHub</a>.</p>
