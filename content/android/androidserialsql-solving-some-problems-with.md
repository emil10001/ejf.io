{
    "slug": "androidserialsql-solving-some-problems-with",
    "date": "2013-09-04T16:26:00.000Z",
    "tags": [
        "android",
        "android development",
        "sqlite"
    ],
    "title": "AndroidSerialSQL - solving some problems with SQLite in Android",
    "publishdate": "2013-09-04T16:26:00.000Z"
}AndroidSerialSQL - solving some problems with SQLite in Android
===============================================================




<p>I created an Android project library that intends to solve the problem of concurrent write attempts from different threads to an Android SQLite database. <a href="https://github.com/emil10001/AndroidSerialSQL" target="_blank">Get the project here</a>.</p>

<h3>Quick Edit</h3>

<p>This solution is intended to solve a specific problem, not to be used in the general case. In a larger, more complex application, you might have data coming in from different sources asynchronously (on different threads), being written to different tables, at random times. In such an application, there is the possibility for two simultaneous writes, where one may silently fail due to a database lock. This solution aims to prevent that from happening. This is not intended for the small application with infrequent database writes, or in cases where you think that being locked out on writes is extremely unlikely, in those cases, it would be better to bundle your writes in batches and transactions, which you should try to do within this framework as well.</p>

<h2>The Problem</h2>

<p>Here&rsquo;s a <a href="http://touchlabblog.tumblr.com/post/24474398246/android-sqlite-locking" target="_blank">blog post explaining the issue</a>, along with a choice quote:</p>

<blockquote>
  <p>If you try to write to the database from actual distinct connections at the same time, one will fail.  It will not wait till the first is done and then write.  It will simply not write your change.  Worse, if you don’t call the right version of insert/update on the SQLiteDatabase, you won’t get an exception.  You’ll just get a message in your LogCat, and that will be it.</p>
</blockquote>

<p>This goes farther than the singleton pattern of only getting one database object (or one writable database object) and implements a blocking queue with a thread pool executor, where the thread pool has a max size of one. This means that there will be a single thread that handles database write operations, and it will work through the backlog of requests that exists in the queue.</p>

<p>In order to accomplish this, we cut off access to a writable version of the database outside of a couple abstract runnables, which are intended to be added to the queue. <code>WriterTask</code> and <code>UpgradeRunnable</code> are those specialized runnables, both of them hold a reference to a database, and have ways of grabbing a reference to the writable db. There are a couple of data structures dedicated to handling the database (or databases), and these special runnables.</p>

<h2>Using this lib</h2>

<p>This is a standard Android library project, so if you&rsquo;re using Eclipse, or are familiar with using Android library projects, just do what you normally do. I should probably turn this into a jar at some point, but I&rsquo;m lazy, and may not get around to it. Plus, if I did that, I&rsquo;d want to make sure that it was polished enough to submit to Maven Central and all that jazz, but that&rsquo;s not where things are right now.</p>

<p><strong>Use at your own risk!</strong> Right now, this is more of a good starting point, and something to look at as a reference. Don&rsquo;t pull it into your project unless you plan on forking it and fixing problems as they appear.</p>

<p>If you&rsquo;re using Gradle, you can do the following in the parent directory, or wherever you want to put your libraries:</p>

<pre><code>git submodule add git@github.com:emil10001/AndroidSerialSQL.git
</code></pre>

<p>In your project parent&rsquo;s <code>settings.gradle</code>:</p>

<pre><code>include ':YourApp', ':AndroidSerialSQL'
</code></pre>

<p>In your app&rsquo;s <code>build.gradle</code>:</p>

<pre><code>dependencies {
    compile project(':AndroidSerialSQL')
}
</code></pre>

<h2>Usage</h2>

<p>There are a few steps to get this up and running. It should all be fairly straght-forward.</p>

<h3>1</h3>

<p>Create a defenition of your database.</p>

<pre><code>DefineDB myDB = new DefineDB("myDB", 1);
myDB.setTableDefenition("items", 
  "create table items "
  + "( _id integer primary key autoincrement, "
  + "item text);");
</code></pre>

<h3>2</h3>

<p>Use the defenition to open/create the database, and store it in a data structure for use.</p>

<pre><code>AccessDB.addDB(context, myDB);
</code></pre>

<h3>3</h3>

<p>Insert an item into your database.</p>

<pre><code>AccessDB.addWriteTask(new WriterTask("myDB", callback) {
@Override
    public void run() {
        db.beginTransaction();
        try {
            ContentValues values = new ContentValues();
            values.put("item", "five");
            db.insert(ITEMS, null, values);
            db.setTransactionSuccessful();
        } catch (Exception ex) {
            Log.e(TAG, "failed to insert", ex);
        } finally {
            db.endTransaction();
        }
        callback.run();
    }
});
</code></pre>

<h3>4</h3>

<p>Retrieve things from the database.</p>

<pre><code>AccessDB.getReadableDB("myDB").query("items", null, 
  null, null, null, null, null);
</code></pre>

<h3>5</h3>

<p>Handle upgrades by adding to the database defenition.</p>

<pre><code>myDB.setVersionUpgrade(2, new UpgradeRunnable() {
    @Override
    public void run() {
        db.execSQL("create table two"
        + "( _id integer primary key autoincrement, "
        + "different_thing text);");
    }
});
</code></pre>

<h2>Sample implementation</h2>

<p>Check out the <a href="https://github.com/emil10001/AndroidSerialSQL/tree/sample" target="_blank">sample branch</a> for a working app that implements this library.</p>

<h2>Get the project</h2>

<p><a href="https://github.com/emil10001/AndroidSerialSQL" target="_blank">Here&rsquo;s the repo on GitHub</a>.</p>
