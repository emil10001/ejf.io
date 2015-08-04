{
    "slug": "playing-around-with-otto-on-android",
    "date": "2013-04-24T04:12:00.000Z",
    "tags": [
        "android",
        "programming",
        "otto",
        "square"
    ],
    "title": "Playing around with Otto on Android",
    "publishdate": "2013-04-24T04:12:00.000Z"
}Playing around with Otto on Android
===================================




<p>There are a few nice Open Source libraries for Android that Square has published. One of them is <a href="http://square.github.io/otto/" target="_blank">Otto</a>, which is a Guava-based event bus system that uses annotations and code generation. Basically, it saves you from needing to create a bunch of interfaces and register them between various parts of your code. Instead, all you need to do is register with the singleton bus, and subscribe to events.</p>

<p>Here&rsquo;s the sample bus provider that Square published with its sample app:</p>

<script src="https://gist.github.com/emil10001/5449473.js?file=BusProvider.java" type="text/javascript"></script><p>Next up, register with the bus. This can be done in your Activity&rsquo;s onCreate, or better register it in onResume, and unregister onPause:</p>

<script src="https://gist.github.com/emil10001/5449473.js?file=RegisterWithBus.java" type="text/javascript"></script><p>Events can be published on the bus using the &lsquo;post&rsquo; method. The object type and the subscribers determine automatically where your event gets routed. Here&rsquo;s an example showing an event being fired when a button is clicked:</p>

<script src="https://gist.github.com/emil10001/5449473.js?file=FireEvent.java" type="text/javascript"></script><p>Subscribing to events is very simple. Again, the object being passed is what determines the event routing.</p>

<script src="https://gist.github.com/emil10001/5449473.js?file=HandleEvent.java" type="text/javascript"></script><p>Full code on <a href="https://github.com/emil10001/OttoExampleAndroid" target="_blank">github</a>.</p>
