{
    "slug": "very-simple-example-of-a-loader-and-loadermanager",
    "date": "2013-09-05T03:42:37.000Z",
    "tags": [
        "android",
        "android development"
    ],
    "title": "Very simple example of a Loader and LoaderManager in Android",
    "publishdate": "2013-09-05T03:42:37.000Z"
}Very simple example of a Loader and LoaderManager in Android
============================================================




<p>Loaders in Android are great, they allow you to asynchronously load data to be used in Adapters. I have found CursorLoaders very useful for getting data from a database to the UI in a way that minimizes blocking calls on the UI thread.</p>

<p>Alex Lockwood has posted a great, and <a href="http://www.androiddesignpatterns.com/2012/07/loaders-and-loadermanager-background.html" target="_blank">detailed four part discussion of the Loader pattern on his blog</a>. If you don&rsquo;t know much about Loaders, or the LoaderManager, or why you might want to use them, go read that series. This is the nickle tour. My goal here is to show you just what you need to get you up and running with this.</p>

<h2>Loaders are simple</h2>

<p>All you really need to do is move your query into the <code>loadInBackground</code> method, and the rest is boilerplate.</p>

<script src="https://gist.github.com/emil10001/6445651.js?file=DumbLoader.java"></script><h2>Something to call back to</h2>

<p>You&rsquo;ll need to put the <code>LoaderCallbacks</code> somewhere, you can put it where you like, in an Activity or Fragment, or in a separate class. They just need to go somewhere.</p>

<script src="https://gist.github.com/emil10001/6445651.js?file=LoaderCallbacks_Cursor.java"></script><h2>Call it!</h2>

<p>Now that you&rsquo;ve got the Loader and the LoaderCallbacks defined, you can call it, and start benefiting from painless asynchronous loading of your data.</p>

<script src="https://gist.github.com/emil10001/6445651.js?file=initialize.java"></script><h2>Sample</h2>

<p>I added a Loader to the same sample project that I <a href="http://www.recursiverobot.com/post/60276620485/androidserialsql-solving-some-problems-with-sqlite-in" target="_blank">blogged about in my previous post</a>. <a href="https://github.com/emil10001/AndroidSerialSQL/tree/sample" target="_blank">Here&rsquo;s the source</a>.</p>
