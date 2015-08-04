{
    "slug": "android-sqlite-locking",
    "date": "2013-09-04T16:03:27.000Z",
    "tags": [
        "android",
        "android development",
        "sqlite"
    ],
    "title": "Android Sqlite Locking",
    "publishdate": "2013-09-04T16:03:27.000Z"
}Android Sqlite Locking
======================




<p><a href="http://touchlabblog.tumblr.com/post/24474398246/android-sqlite-locking" class="tumblr_blog" target="_blank">touchlabblog</a>:</p>

<blockquote>
<p>Recently I’ve been doing quite a bit of work with the Android Sqlite database.  Mostly with the android piece of <a href="http://ormlite.sourceforge.net/" target="_blank">ormlite</a>.</p>
<p>The Android examples cover some basic Sqlite usage, but they really don’t go into depth with regards to proper usage patters, and more importantly, improper usage patters.  Most examples and documentation is slated towards using very basic database queries, and beyond that, creating a ContentProvider.  What never really seems to be covered is stuff like:</p>
<ul><li>Where do you create and store your SQLiteOpenHelper instances?</li>
<li>How many should you have?</li>
<li>Are there any concerns when accessing the database from multiple threads?</li>
</ul><p>If you look around for information you’ll find a lot of partial or incorrect info.  A great example was forwarded to me by Gray yesterday (he runs the ormlite project).  It was on stackexchange…</p>
<p><a href="http://stackoverflow.com/questions/2493331/what-is-best-practice-with-sqlite-and-android/2493839" target="_blank"></a><a href="http://stackoverflow.com/questions/2493331/what-is-best-practice-with-sqlite-and-android/2493839" target="_blank">http://stackoverflow.com/questions/2493331/what-is-best-practice-with-sqlite-and-android/2493839</a></p>

<p><a href="http://touchlabblog.tumblr.com/post/24474398246/android-sqlite-locking" target="_blank">Read More</a></p></blockquote>

<p>This is a great post on Android SQLite, and definitely worth taking the time to read. I first found it a year ago on a different blog, the post seems to move around once in a while, so grab an offline copy for safe-keeping.</p>
