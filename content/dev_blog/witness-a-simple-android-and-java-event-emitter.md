{
    "slug": "witness-a-simple-android-and-java-event-emitter",
    "date": "2014-05-19T15:18:00.000Z",
    "tags": [
        "java",
        "android",
        "Android Development",
        "Google Glass",
        "googleglass",
        "gdk",
        "GDK"
    ],
    "title": "Witness, a simple Android and Java Event Emitter",
    "publishdate": "2014-05-19T15:18:00.000Z"
}Witness, a simple Android and Java Event Emitter
================================================




<p><img src="https://31.media.tumblr.com/1b64e007a69f0146c1dc820b942baf3b/tumblr_inline_n5tv0gM77r1sq0x3a.jpg" alt=""/></p>

<blockquote>
  <p><a href="https://secure.flickr.com/photos/nhn_2009/5174693873/?rb=1" target="_blank">Source</a>. I found this in an image search for &lsquo;witness&rsquo;. Had to use it. =)</p>
</blockquote>

<p>I&rsquo;ve been working on a project for Google Glass and Android that requires asynchronous messages to be passed around between threads. In the past, I&rsquo;ve used Square&rsquo;s Otto for this, but I wasn&rsquo;t thrilled with the performance. Additionally, Otto makes some assumptions about which thread to run on, and how many threads to run with, that I wasn&rsquo;t crazy about. Before continuing, I should point out that Otto is configurable, and some of these basic assumptions can be dealt with in the configs. Regardless, I felt like writing my own.</p>

<h3>Code</h3>

<p><a href="https://github.com/emil10001/Witness" target="_blank">Witness</a> is a really simple event emitter for Java. The idea here is to build a really simple structure that can be used to implement observers easily. It&rsquo;s sort of like JavaScript&rsquo;s Event Emitter, and the code is short enough to read in its entirety.</p>

<p>I&rsquo;ll start with <code>Reporter</code> because it&rsquo;s so simple. Essentially, this is just an interface that you implement if you want your class to be able to receive events.</p>

<script src="https://gist.github.com/emil10001/027fa3b3693db7cc10e0.js?file=Reporter.java"></script><p>The <code>Witness</code> class maps class types to <code>Reporter</code> instances. This means that for a given data type, <code>Witness</code> will fan out the event to all registered <code>Reporter</code>s. It uses an <code>ExecutorService</code> with a pool size of 10 threads to accomplish this as quickly as possible off of the main thread:</p>

<script src="https://gist.github.com/emil10001/027fa3b3693db7cc10e0.js?file=Witness.java"></script><h3>Usage</h3>

<p>To receive events for a specific datatype, the receiving class needs to implement the <code>Reporter</code>
interface, then register for whatever data types the following way:</p>

<pre><code>Witness.register(EventTypeOne.class, this);
Witness.register(EventTypeTwo.class, this);
</code></pre>

<p>When you&rsquo;re done listening, unregister with the following:</p>

<pre><code>Witness.remove(EventTypeOne.class, this);
Witness.remove(EventTypeTwo.class, this);
</code></pre>

<p>To publish events to listeners:</p>

<pre><code>Witness.notify(event);
</code></pre>

<p>Handling events in the <code>Reporter</code>:</p>

<pre><code>@Override
public void notifyEvent(Object o) {
    if (o instanceof SomeObject) {
        objectHandlingMethod(((SomeObject) o));
    }
}
</code></pre>

<p>Android, it is a good idea to use a handler, to force the event handling to run on the thread you expect. E.g., if you need code run on the main thread, in an Activity or Service:</p>

<pre><code>public class MyActivity extends Activity implements Reporter {
    private Handler handler = new Handler();

    // ...

    @Override
    public void notifyEvent(final Object o) {
        handler.post(new Runnable() {
            @Override
            public void run() {

                if (o instanceof SomeObject) {
                    objectHandlingMethod(((SomeObject) o));
                }

            }
        });
    }
}
</code></pre>

<h3>Notes</h3>

<p>Events are published on a background thread, using an <code>ExecutorService</code>, backed by a <code>BlockingQueue</code>.
This has a few important implications:</p>

<ul><li>Thread safety

<ul><li>You need to be careful about making sure that whatever you&rsquo;re using this for is thread safe</li>
</ul></li>
<li>UI/Main thread

<ul><li>All events will be posted to background threads</li>
</ul></li>
<li>Out of order

<ul><li>Events are handled in parallel, so it is possible for them to come in out of order</li>
</ul></li>
</ul><p>Please find this project on <a href="https://github.com/emil10001/Witness" target="_blank">GitHub</a>.  If I get enough questions about it, I might be willing to take the time to package it and submit it to maven central.</p>

<h3>Update</h3>

<p><a href="https://plus.google.com/u/0/115844465204731828022" target="_blank">+Dietrich Schulten</a> had the following <a href="https://plus.google.com/u/0/+EJohnFeig/posts/eqprQafTTLc" target="_blank">comment</a>:</p>

<blockquote>
  <p>It has the effect that events can be delivered on arbitrary threads. The event handler must be threadsafe, must synchronize access to shared or stateful resources, and be careful not to create deadlocks. Otoh if you use a single event queue, you avoid this kind of complexity in the first place. I&rsquo;d opt for the latter, threadsafe programming is something you want to avoid if you can.﻿</p>
</blockquote>

<p>I should note that my usage is all on Android, where I&rsquo;m explicitly specifying the thread that the events will run on using <code>Handler</code>s. I haven&rsquo;t used this in a non-Android environment, and I&rsquo;m not entirely sure how to implement the equivalent behavior for regular Java.</p>
