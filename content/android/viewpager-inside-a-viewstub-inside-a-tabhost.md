{
    "slug": "viewpager-inside-a-viewstub-inside-a-tabhost",
    "date": "2013-06-26T00:23:00.000Z",
    "tags": [
        "android",
        "android development",
        "programming",
        "development"
    ],
    "title": "ViewPager inside a ViewStub inside a TabHost inside a ViewStub",
    "publishdate": "2013-06-26T00:23:00.000Z"
}ViewPager inside a ViewStub inside a TabHost inside a ViewStub
==============================================================




<p>This is a wacky experiment to hash out what&rsquo;s necessary to create some views. I had some specific needs in a project that I&rsquo;m working on. I needed a TabHost within a ViewStub, which contained ViewStubs, when inflated became ViewPagers. I also didn&rsquo;t want to use fragments for this, so this is all done with normal views. It should be a fairly lightweight strategy. I populated it with a bunch of images, so those will chew up quite a bit of heap space, but the app should launch quickly, and be generally responsive.</p>

<p>There are no dependencies, and no permissions used for this. Feel free to <a href="https://github.com/emil10001/TvFan-ViewPagerSample" target="_blank">pull down the code</a> and run it yourself, this whole thing will make a lot more sense once you do. As I said, the code that I&rsquo;ll be discussing was written as a sample implementation for another project that I am working on. Therefore, I didn&rsquo;t spend much time optimizing it, or flattening out the hierarchy as much as I could have. There are some obvious questions that I have about things that might be removed, and I&rsquo;ll try to note them as we go.</p>

<h2>Pretty screenshots</h2>

<p>In case you are too lazy, or can&rsquo;t, for some reason, install the app, here&rsquo;s a few screenshots that might illuminate things for you:</p>

<p><img src="http://files.feigdev.com/vp_screens/1.png" width="200px" alt="screenshot"/> . 
<img src="http://files.feigdev.com/vp_screens/2.png" width="200px" alt="screenshot"/><br/><img src="http://files.feigdev.com/vp_screens/3.png" width="200px" alt="screenshot"/> . 
<img src="http://files.feigdev.com/vp_screens/4.png" width="200px" alt="screenshot"/></p>

<p>One thing to note from the above images is that swiping between pages does not change the current tab.</p>

<h2>Main layout</h2>

<p>Let&rsquo;s start by looking at the main layout file, since that&rsquo;s usually a good place to start, and will generally give you a rough idea of what you might expect to see on screen.</p>

<script src="https://gist.github.com/emil10001/5863269.js?file=activity_main.xml"></script><p>As you can see, there&rsquo;s not much here. We&rsquo;ve got a toggle button and a couple of empty views. The FrameLayout view will be used what gets set to VISIBLE/GONE by the toggle button. I probably could&rsquo;ve ditched that, and used a view inside the ViewStub, but this was slightly easier to understand, slightly. Inside the FrameLayout, you&rsquo;ll see a ViewStub; this allows us to inflate another layout in that spot whenever we need it. It&rsquo;s cheap and light-weight, because you&rsquo;re not inflating anything until it&rsquo;s needed, so when the view is initially built, this will be empty until you inflate it.</p>

<h2>Inside the ViewStub</h2>

<p>If you look at the layout parameter defined in the ViewStub, it&rsquo;s specifying another layout file called tabstub.xml. Let&rsquo;s look at that.</p>

<script src="https://gist.github.com/emil10001/5863269.js?file=tabstub.xml"></script><p>This layout is a bit more interesting, as there are more things going on here. Mainly though, it&rsquo;s just a standard tab layout where the tabs are not the parent view (not sure if this is strictly necessary), and where the contents of each tab are ViewStubs.</p>

<h2>Tab layout</h2>

<p>The layout for the individual tab, once inflated, is pretty simple, it&rsquo;s just a ViewPager that we&rsquo;ll be programmatically inflating and adding more views to.</p>

<script src="https://gist.github.com/emil10001/5863269.js?file=tab_one.xml"></script><h2>Layouts</h2>

<p>Here we have the object that&rsquo;s going to keep track of all of the layouts in all of the ViewPagers across all the tabs. It&rsquo;s an enum, so that the logic will be a bit cleaner. Each of the enum&rsquo;s constants represents a tab, and contains an array of layout references.</p>

<script src="https://gist.github.com/emil10001/5863269.js?file=Layouts.java"></script><h2>Individual page</h2>

<p>Here&rsquo;s what one of the pages looks like, couldn&rsquo;t be any simpler.</p>

<script src="https://gist.github.com/emil10001/5863269.js?file=alf.xml"></script><h2>Starting out in the code</h2>

<p>When we first launch this app, the view is blank, except for a toggle button. We need to click the button to get started. Here&rsquo;s what gets run when you click &lsquo;toggle&rsquo;.</p>

<script src="https://gist.github.com/emil10001/5863269.js?file=toggleTabs.java"></script><p>This does a couple of things, first it needs to initialize the tabs, in case they haven&rsquo;t yet been inflated. Second, it sets the visibility of the view that contains the tabs.</p>

<h2>Initializing the tabs</h2>

<p>When we clicked the toggle button, the first thing that happened was that we needed to initialize the tabs. This means both inflating the views, as well as setting the TabHost&rsquo;s contents.</p>

<script src="https://gist.github.com/emil10001/5863269.js?file=initTabs.java"></script><p>If the tabs are already initialized, we don&rsquo;t need to do anything. Otherwise, we need to build them, and inflate the first ViewPager, since that&rsquo;s visible. This is done with <code>initPager()</code>.</p>

<h2>Initializing the ViewPagers</h2>

<p>Each tab has its own custom instance of ViewPager, and layout file. This method simply associates the two.</p>

<script src="https://gist.github.com/emil10001/5863269.js?file=initPager.java"></script><p>When we swipe between pages in the ViewPager, we need to make sure that the layouts for each page are inflated, so we override <code>instantiateItem</code> in our PagerAdapter.</p>

<script src="https://gist.github.com/emil10001/5863269.js?file=instantiateItem.java"></script><h2>What happens when tabs are clicked</h2>

<p>When tabs are clicked, we need to make sure that each ViewPager is initialized, just like the first one.</p>

<script src="https://gist.github.com/emil10001/5863269.js?file=onTabChanged.java"></script><h2>Conclusion</h2>

<p>While I&rsquo;m still in the middle of implementing this strategy in my project, the demo worked out perfectly. I&rsquo;m fairly pleased with the performance, though the images used do chew up quite a bit of memory. If you&rsquo;re looking at DDMS when this is running, you might see your heap size grow to ~30MB.</p>

<p><a href="https://github.com/emil10001/TvFan-ViewPagerSample" target="_blank">Source on GitHub</a>.</p>
