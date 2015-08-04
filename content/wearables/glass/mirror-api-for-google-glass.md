{
    "slug": "mirror-api-for-google-glass",
    "date": "2014-02-17T01:00:00.000Z",
    "tags": [
        "glass",
        "googleglass",
        "mirrorapi",
        "mirror api"
    ],
    "title": "Mirror API for Google Glass",
    "publishdate": "2014-02-17T01:00:00.000Z"
}Mirror API for Google Glass
===========================




<p>I&rsquo;m giving a few talks on the Mirror API for Google Glass, and I needed to organize some thoughts on it. I figured that writing a blog post would a good way to do that.</p>

<p>Here is the slide deck for the shared content of my Mirror talks:</p>

<iframe src="https://docs.google.com/presentation/d/1FzGVnzhBLFaaz4wkxTvkzWiC8mpZuM2bMVgqMPZCp98/embed?start=false&amp;loop=false&amp;delayms=3000" frameborder="0" width="480" height="299" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

<p>Since I don&rsquo;t want to miss anything, I&rsquo;m going to follow the structure of the slides in this post. What I am not going to do, is be consistent with languages. I&rsquo;ll be bouncing between Java and JavaScript, with bits of Android flavored Java thrown in for good measure. It really just comes down to what I have code for right now in which language. The goal here is to discuss the concepts, and to get a good high-level view.</p>

<h2>What is the Mirror API?</h2>

<h3>What is it?</h3>

<p>The Mirror API is one API available for creating Glassware. The main other option is the GDK. Mirror is REST based, mostly done server-side; it pushes content to Glass, and may listen for Glass to respond. The GDK is client-side, and is similar to programming for Android. The general rule of thumb, is that if you can do what you need to do with Mirror, that&rsquo;s the right choice. Mirror is a bit more constrained, and is likely to deliver a good user experience, without a lot of effort. It also allows Glass to manage execution in a battery friendly way.</p>

<h3>What does it do?</h3>

<p>Mirror handles push messages from servers, and responds as necessary to them. The basic case is that your server pushes a message to Glass, then the message displayed on Glass as a Timeline Card.</p>

<h3>How does it work?</h3>

<p>If you&rsquo;re coming from the Android world then you might be familiar with GCM, and push messages. Mirror works using GCM, similar to how Android uses GCM.</p>

<p>You can send messages to Glass, and receive messages from Glass. Sending messages is much easier than receiving messages. On the sending side, all you need is a network connection, and the ability to do the OAuth dance. If you want to receive data from Glass, you&rsquo;ll also need some way for Glass to reach you, via a callback URL.</p>

<h3>What’s the flow?</h3>

<p>Glass registers with Google&rsquo;s servers, and then your server sends requests to Google&rsquo;s servers, and Google&rsquo;s servers communicate with Glass. You can send messages to Google&rsquo;s servers, and you can receive messages back from Glass via callback URLs that you provide. With that, you can get a back and forth going.</p>

<h3>SO MANY QUESTIONS?!?!?!</h3>

<p>I know, there&rsquo;s a lot here. We&rsquo;ll try to step through as much as possible in this post.</p>

<h2>Setting up</h2>

<p>Before you do anything else, you&rsquo;ll need to head over to the <a href="https://cloud.google.com/console/project" target="_blank">Google Developer Console</a>, and register your app. Create a new app, turn on &lsquo;Mirror API&rsquo;, and turn off everything else. Then, go into the Credentials section, so that you can get your Client Id and Client Secret. This is outlined pretty clearly in the <a href="https://developers.google.com/glass/develop/mirror/quickstart/java" target="_blank">Glass Quick Start documentation</a>.</p>

<h2>OAuth</h2>

<p>To communicate with Glass, you&rsquo;ll first need to do do the OAuth dance. The user visits your website, and clicks a link that will ask them to authenticate with one of their Google accounts. It then asks for permission to update their Mirror Timeline. After that&rsquo;s done, you&rsquo;ll receive a request at the callback URL, with a code. The code needs to be exchanged for a token. Once you have the token, you can begin making authenticated requests. The token will expire, so you&rsquo;ll need to be able to re-request it when it does.</p>

<p>Further reading in the <a href="https://developers.google.com/glass/develop/mirror/quickstart/java" target="_blank">Quick Start</a>.</p>

<h2>Timeline</h2>

<p>The timeline is the collection of Cards that you have on Glass, starting from the latest, and going back into the past. It also includes pinned cards, and things happening 'in the future&rsquo;, provided by Google Now.</p>

<p>Here&rsquo;s a video explanation from the Glass Team:</p>

<iframe width="560" height="315" src="//www.youtube.com/embed/qfs5d00TNrA" frameborder="0" allowfullscreen></iframe>

<h3>Why a Timeline?</h3>

<p>Well, you&rsquo;d have to ask the Glass team, why exactly a Timeline, but I could venture a guess. One of Glass&rsquo;s issues is that it is more difficult to navigate with. You can&rsquo;t dive into apps like you can on your phone, at least not quickly and easily. There&rsquo;s no 'Recent Apps&rsquo; section, no home screen and no launcher (well, except from the voice commands).</p>

<p>Launching apps with voice commands allows you to <em>do</em> things, as opposed to going to an app and poking around. For example, saying <code>OK Glass, start a bike ride</code> launches Strava into a mode where it immediately begins tracking me with GPS and giving me feedback on that tracking. It does not let me look at my history there.</p>

<p>Glass tends to operate on push rather than pull. Glassware surfaces information to you, as it becomes relevant. This ability allows Glass to become incredibly contextual, though we have only seen bits of this so far with Google Now. Imagine a contextual Glassware that keeps a todo list of all of your tasks, both work and home. It could surface a Card when you get to your office with the highest priority work tasks that you have for that day. On the weekend, after you&rsquo;re awake, it could tell you that you need to get your car into the shop to get your smog check.</p>

<p>A typical category of Glassware today is news, which all work well with a push approach. Now, this is not terribly useful on Glass, as it&rsquo;s difficult to consume, but there are some good ones that have figured out how to deliver the content in a way that is pleasant to consume on Glass (Unamo, and CNN), or to allow you to save it off to Pocket for picking up later (Winkfeed).</p>

<p>The timeline is semi-ephemeral. Your cards will stick around for 7 days, and will be lost after that. Before you get all clingy and nostalgic, consider that that information is 7 days old, and will not only be stale, but difficult to even scroll to at that point. I have rarely gone back more than a day in my history to dig for something that I saw, and that was usually for a photo. Cards are usually a view into a service, so the information is rarely lost to the ether. Instead of scrolling for a solid minute trying to find something, you can pull out your phone and go to the app on your phone. Like G+ Photos, Gmail or Hangouts. Apps like Strava don&rsquo;t even put your recent rides in your timeline, you have to either go to the app or their website for history.</p>

<h3>What are Cards?</h3>

<p>Cards are the individual items in the timeline. Messages that you send from your server to Glass users typically end up as timeline cards on the user&rsquo;s Glass. There are a number of basic actions, as well as custom actions, that users can take on cards, depending on what the card supports. How this works will become apparent once we look at the code.</p>

<h3>How do I insert Cards into the Timeline?</h3>

<p>Using the API, of course! Let&rsquo;s look at how to do this in a couple of languages.</p>

<p>Java:</p>

<pre><code>TimelineItem timelineItem = new TimelineItem();
timelineItem.setText("Hello World");
Mirror service = new Mirror.Builder(new NetHttpTransport(), new JsonFactory(), null).setApplicationName(appName).build();
service.timeline().insert(timelineItem).setOauthToken(token).execute();
</code></pre>

<p>JavaScript:</p>

<pre><code>client
    .mirror.timeline.insert(
    {
        "text": "Hello world",
        "menuItems": [
            {"action": "DELETE"}
        ]
    }
)
.withAuthClient(oauth2Client)
.execute(function (err, data) {
    if (!!err)
        errorCallback(err);
    else
        successCallback(data);
});
</code></pre>

<p>Further reading in the <a href="https://developers.google.com/glass/develop/mirror/timeline" target="_blank">Developer Guides</a>, and <a href="https://developers.google.com/glass/v1/reference/timeline" target="_blank">Reference</a>.</p>

<h2>Locations</h2>

<p>Location with Glass can mean one of two things, either you&rsquo;re pushing a location to Glass, so that the user can use it to navigate somewhere. Or, you&rsquo;re requesting the user&rsquo;s location from Glass. The Field Trip Glassware subscribes to the user&rsquo;s location so that it can let the user know about interesting things in the area.</p>

<h3>Sending Locations to Glass</h3>

<p>Sending locations is very simple. Basically, you send a timeline card with a location parameter, and add the <code>NAVIGATION</code> menu item.</p>

<p>JavaScript:</p>

<pre><code>client
    .mirror.timeline.insert(
    {
        "text": "Let's meet at the Hacker Dojo!",
        "location": {
            "kind": "mirror#location",
            "latitude": 37.4028344,
            "longitude": -122.0496017,
            "displayName": "Hacker Dojo",
            "address": "599 Fairchild Dr, Mountain View, CA"
        },
        "menuItems": [
            {"action":"NAVIGATE"},
            {"action": "REPLY"},
            {"action": "DELETE"}
        ]
    }
)
    .withAuthClient(oauth2Client)
    .execute(function (err, data) {
        if (!!err)
            errorCallback(err);
        else
            successCallback(data);
    });
</code></pre>

<p>I&rsquo;m using this right now for something very simple. In a Mirror/Android app that I&rsquo;m working on, I can share locations from Maps to Glass, and then use Glass for navigation. Nothing is more annoying than having Glass on and then using your phone for navigation because the street is impossible to pronounce correctly.</p>

<h3>Getting Locations from Glass</h3>

<p>This is a little more tricky. First, you need to add a permission to your scope, <code><a href="https://www.googleapis.com/auth/glass.location" target="_blank">https://www.googleapis.com/auth/glass.location</a></code>. Getting locations back from Glass is going to require you to have a real server that is publicly accessible. It should also support SSL requests if you&rsquo;re in production.</p>

<p>Here&rsquo;s some code from the Java Quick Start that deals with handling location updates:</p>

<pre><code>LOG.info("Notification of updated location");
Location location = MirrorClient.getMirror(credential).locations().get(notification.getItemId()).execute();

LOG.info("New location is " + location.getLatitude() + ", " + location.getLongitude());
MirrorClient.insertTimelineItem(
    credential, 
    new TimelineItem().setText("Java Quick Start says you are now at " 
        + location.getLatitude()
        + " by " + location.getLongitude())
    .setNotification(new NotificationConfig().setLevel("DEFAULT"))
    .setLocation(location).setMenuItems(Lists.newArrayList(new MenuItem().setAction("NAVIGATE"))));
</code></pre>

<p>This will really help enable contextual Glassware. You could imagine Starbucks Glassware that offers you a discounted coffee when you get near a Starbucks. Or a Dumb Starbucks Glassware that does the same thing, but claims to be an art installation.</p>

<p>Further reading in the <a href="https://developers.google.com/glass/develop/mirror/location" target="_blank">Developer Guides</a>, and <a href="https://developers.google.com/glass/v1/reference/locations" target="_blank">Reference</a>.</p>

<h2>Menus</h2>

<p>Menus are the actions that a user can take on a card. They make cards interactive. The most basic one is <code>DELETE</code>, which, obviously, allows the user to delete a card. Other useful actions are <code>REPLY</code>, <code>READ_ALOUD</code>, and <code>NAVIGATE</code>.</p>

<p>You can see menu items added to cards in previous examples. I will repeat the JavaScript navigation example here:</p>

<p>JavaScript:</p>

<pre><code>client
    .mirror.timeline.insert(
    {
        "text": "Let's meet at the Hacker Dojo!",
        "location": {
            "kind": "mirror#location",
            "latitude": 37.4028344,
            "longitude": -122.0496017,
            "displayName": "Hacker Dojo",
            "address": "599 Fairchild Dr, Mountain View, CA"
        },
        "menuItems": [
            {"action":"NAVIGATE"},
            {"action": "REPLY"},
            {"action": "DELETE"}
        ]
    })
    .withAuthClient(oauth2Client)
    .execute(function (err, data) {
        if (!!err)
            errorCallback(err);
        else
            successCallback(data);
    });
</code></pre>

<p>The Card generated from the example should allow you to get directions to the Hacker Dojo. You can also add custom menus, like Winkfeed&rsquo;s incredibly useful <code>Save to Pocket</code> menu item.</p>

<p>Further reading in the <a href="https://developers.google.com/glass/develop/mirror/menu-items" target="_blank">Developer Guides</a>, and <a href="https://developers.google.com/glass/v1/reference/timeline" target="_blank">Reference</a>.</p>

<h2>Subscriptions</h2>

<p>Subscriptions allow you to get data back from the user. This can happen automatically, if you&rsquo;re subscribed to the user&rsquo;s locations, or when the user takes some action on a card that is going to interact with your service. E.g., you can subscribe to <code>INSERT</code>, <code>UPDATE</code>, and <code>DELETE</code> notifications. You will also get notifications if you support the user replying to a card with <code>REPLY</code>.</p>

<p>In Java:</p>

<pre><code>// Subscribe to timeline updates
MirrorClient.insertSubscription(credential, WebUtil.buildUrl(req, "/notify"), userId, "timeline");
</code></pre>

<p>Subscriptions require a server, and cannot be done with localhost. However, <a href="https://ngrok.com/" target="_blank">tools like ngrok</a> may be used to get a publicly routable address for your localhost. I have heard that ngrok does not play nice with Java, but you might be able to find another tool that does.</p>

<p>Among other things, you can learn from these sorts of subscriptions. You can learn which types of Cards get deleted the most, for example. Subscriptions can also listen for custom menus, voice commands and locations. With Evernote, you can say, <code>take a note</code> and it will send the text of your note to the Evernote Glassware.</p>

<p>Further reading in the <a href="https://developers.google.com/glass/develop/mirror/subscriptions" target="_blank">Developer Guides</a>, and <a href="https://developers.google.com/glass/v1/reference/subscriptions" target="_blank">Reference</a>.</p>

<h2>Contacts</h2>

<p>Another important type of subscription is subscribing for <code>SHARE</code> notifications. The <code>SHARE</code> action also involves Contacts.</p>

<p>Contacts are endpoints for content. They can be Glassware or people, though if they&rsquo;re people, they are probably still really some sort of Glassware that maps to people. Sharing to Contacts allow your Glassware to receive content from other Glassware. You can insert a Contact into a user&rsquo;s time like so (in JavaScript):</p>

<pre><code>client
    .mirror.contacts.insert(
    {
        "id": "emil10001",
        "displayName": "emil10001",
        "iconUrl": "https://secure.gravatar.com/avatar/bc6e3312f288a4d00ba25500a2c8f6d9.png",
        "priority": 7,
        "acceptCommands": [
            {"type": "POST_AN_UPDATE"},
            {"type": "TAKE_A_NOTE"}
        ]
    })
    .withAuthClient(oauth2Client)
    .execute(function (err, data) {
        if (!!err)
            errorCallback(err);
        else
            successCallback(data);
    });
</code></pre>

<p>It&rsquo;s obvious what you want people Contacts for, but what about Glassware contacts? Well, imagine sharing an email with Evernote, or a news item with Pocket. Now, consider that as the provider of the email or news item, you don&rsquo;t need to provide the Evernote or Pocket integration yourself, you just need to allow sharing of that content. It&rsquo;s a very powerful idea, analogous to Android&rsquo;s powerful sharing functionality.</p>

<p>Further reading in the <a href="https://developers.google.com/glass/develop/mirror/contacts" target="_blank">Developer Guides</a>, and <a href="https://developers.google.com/glass/v1/reference/contacts" target="_blank">Reference</a>.</p>

<h2>Code Samples</h2>

<p>I have three sample projects that I have been pulling code from for this post.</p>

<ul><li><a href="https://github.com/emil10001/mirror-quickstart-java" target="_blank">A slightly modified Java Quick Start</a> provided by the Glass Team</li>
<li>My <a href="https://github.com/emil10001/GlassFromAndroid" target="_blank">Android Mirror demo</a></li>
<li>My <a href="https://github.com/emil10001/glass-mirror-nodejs-auth-demo" target="_blank">node.js Mirror demo</a></li>
</ul>
