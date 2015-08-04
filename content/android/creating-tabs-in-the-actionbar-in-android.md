{
    "slug": "creating-tabs-in-the-actionbar-in-android",
    "date": "2013-03-04T02:40:00.000Z",
    "tags": [
        "android",
        "programming",
        "actionbarsherlock"
    ],
    "title": "Creating Tabs in the ActionBar in Android",
    "publishdate": "2013-03-04T02:40:00.000Z"
}Creating Tabs in the ActionBar in Android
=========================================




<p><img src="http://files.feigdev.com/ab_tabs.png" alt="tabs screenshot"/></p>



<p>The following blog post was written over a year ago. The app discussed, Pin&rsquo;d, has since been completed, published, and removed from the Google Play Store. I have decided not to modify the post, because it still contains lots of useful information.</p>



<p>In working on my Pinterest app, <a href="http://pind.feigdev.com/" target="_blank">Pin&rsquo;d</a>, I decided to switch from a list navigation to using tabs. I also wanted the tabs to hang off of the ActionBar, instead of needing to manage them separately, as this would be the least amount of refactoring, and would fit in nicely with the rest of my app. I also ran into a question on the android-developers google group about how to do this. I figured that these two things were reason enough to code up a sample implementation.</p>



<p>The first thing that needs to be done is to set up a project with either ActionBarCompat or ActionBarSherlock. After using both in different projects, I&rsquo;ve settled on ActionBarSherlock, and that&rsquo;s what this post will be referencing. To get ActionBarSherlock into your project, you should <a href="http://actionbarsherlock.com/download.html" target="_blank">download the library</a> and follow the instructions there. If you&rsquo;re feeling really lazy, I&rsquo;ll give you the short version. Download the zip and extract it. Create a new android project in Eclipse, don&rsquo;t automatically create an Activity. Once the project exists, right click it, go to Properties -&gt; Android, check the &lsquo;Is Library&rsquo; checkbox and hit 'ok&rsquo;. Now, right click the project again, select Import -&gt; General -&gt; File System, then select the library directory of the ActionBarSherlock contents you downloaded and import it. Now you can create your new project, calling it whatever you want and letting Eclipse create the default activity for you (if you want). You should have your project in the workspace now, if so, right click it and select Properties -&gt; Android then in the library section, click 'Add&rsquo; and add the ActionBarSherlock library. Before you close that dialog, change the SDK build version to API level 13. Now you&rsquo;re ready to start coding. First step is to add the appropriate bits into your AndroidManifest.xml file. While ActionBarSherlock will build on targets down to API level 7, the target needs to be set at 13. This is because ABS will try to use the native methods wherever possible, and to do that, they need to be accessible to the app. Open up the manifest, and add the following line:</p>

<p><script src="https://gist.github.com/emil10001/590636713d58cbc11b0a.js?file=sdk_target_min" type="text/javascript"></script></p>

<p>ActionBarSherlock requires that a theme be specified in the manifest. This is how the system knows where to apply the ActionBar and what it should look like. Add the following to either the top application or top activity:</p>

<p><script src="https://gist.github.com/emil10001/590636713d58cbc11b0a.js?file=theme" type="text/javascript"></script></p>

<p>Adding this to the application applies it to all Activities, and applying it to the Activity limits the use of ABS to that one Activity. That&rsquo;s all we need to do for the manifest, so go ahead and save it and close it. Next, open up the activity that you created. Mine is called 'SimpleActionBarTabsActivity, and is located here:</p>

<p><script src="https://gist.github.com/emil10001/590636713d58cbc11b0a.js?file=filename" type="text/javascript"></script></p>

<p>When using ActionBarSherlock, the Activity needs to extend FragmentActivity instead of just an Activity. The bonus here is that this opens up fragment support for your app to phones running Android 2.1. Fragments are really useful, and you should consider using them, or at least playing around with them a bit so that you know what they are and how they work. You should have something like this:</p>

<p><script src="https://gist.github.com/emil10001/590636713d58cbc11b0a.js?file=classdef" type="text/javascript"></script></p>

<p>We can actually do most of the work for this in the 'onCreate&rsquo; method, here&rsquo;s what you need to get two tabs listed:</p>

<p><script src="https://gist.github.com/emil10001/590636713d58cbc11b0a.js?file=onCreate" type="text/javascript"></script></p>

<p>Stepping through the above code, the first few lines are typical of just about any app. Take a look at the line that says getSupportActionBar()&hellip;, this is where we are telling the system that we want to use tabs as the default navigation mode. There are a few different navigation modes to choose from, standard, tabs and lists. Standard doesn&rsquo;t give you anything special and list mode will give you a drop-down list in your ActionBar. In the next line, we create a new ActionBar.Tab object, then we set the title of the tab, pretty simple. Then at the bottom, we add the tab to the ActionBar. If you do all this, and run it, you should now see tabs showing up within or below your ActionBar. Try clicking them. They don&rsquo;t do anything, do they? To add the ability to click on the tabs, you will need to modify the class declaration to add 'ActionBar.TabListener&rsquo;:</p>

<p><script src="https://gist.github.com/emil10001/590636713d58cbc11b0a.js?file=updated_classdef" type="text/javascript"></script></p>

<p>ActionBar.TabListener is just like other listeners, but focuses on which tab was selected instead of if a button was clicked or something. The following two lines will need to be added to the 'onCreate&rsquo; method:</p>

<p><script src="https://gist.github.com/emil10001/590636713d58cbc11b0a.js?file=set_listeners" type="text/javascript"></script></p>

<p>And the following three methods will need to be added to the class:</p>

<p><script src="https://gist.github.com/emil10001/590636713d58cbc11b0a.js?file=implement_listeners" type="text/javascript"></script></p>

<p>That is just about it. I&rsquo;m sure that if you play around with this, or just by reading it, you might gather that this code doesn&rsquo;t actually do a whole lot. Currently, selecting a tab just leaves the same content on the screen. But, that is simple enough to fix, just use the three TabListener methods to manage Fragments. Start by adding a fragment, then swap it out using the fragment replace method, or add to and pop off the stack. Although, that is outside of the scope of this tutorial. Feel free to pull down the full <a href="https://github.com/emil10001/SimpleActionBarTabs" target="_blank">github project</a> and play around with it yourself.</p>
