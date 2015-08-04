{
    "slug": "talking-to-glass-from-android-via-the-mirror-api",
    "date": "2013-08-18T17:39:00.000Z",
    "tags": [
        "googleglass",
        "glass",
        "mirrorapi",
        "android",
        "java"
    ],
    "title": "Talking to Glass from Android via the Mirror API",
    "publishdate": "2013-08-18T17:39:00.000Z"
}Talking to Glass from Android via the Mirror API
================================================




<p>This week, I started prototyping a stripped down Java implementation of a Mirror service for <a href="https://play.google.com/store/apps/details?id=com.talkray.client" target="_blank">Talkray, the awesome mobile communications app</a> that I work on at work. The typical flow for installing Mirror Glassware is to visit a website, click an &lsquo;install&rsquo; button, pick your Google account and grant access, and Google returns you back to the website, giving the website your token. This works fine for brand new services, or web services, that already have your account, and are adept at doing OAuth. However, for a mobile app, where the account only lives on the phone and servers that don&rsquo;t serve web pages, this presents a bit of a challenge. I could implement the full OAuth dance server-side, but, if I did that on the web, I&rsquo;d need a way to make sure that we&rsquo;re tying this to the correct account. Then, I had a crazy idea, what if I could use Android&rsquo;s built-in APIs that talk to Google services for talking with Glass? Could this work? Spoiler: the short answer is no, and the long answer is a qualified yes.</p>

<p><a href="https://github.com/emil10001/GlassFromAndroid" target="_blank">Jump straight to the code.</a></p>

<p><img src="https://lh6.googleusercontent.com/-2r5Sw2pK1DY/UgwNL9mgraI/AAAAAAAAzbQ/UGCm3EI34yU/w2436-h1788-no/20130814_155719_973_1.jpg" width="600px"/></p>

<h2>What do you mean, yes and no? Does it work?</h2>

<p>While I have not spent a ton of time with this, there seem to be a few major roadblocks in the way of making this a reasonable solution. First off, <strong>I am able to authenticate and push items to Glass from Android</strong>, and I will get into the details shortly. However, the roadblocks are not insignificant. For one thing, as far as I&rsquo;m aware, I can only initiate requests from Android, I can&rsquo;t handle <code>POST</code> requests to my app. This means that there would be no way to respond to an item on Glass back to my app.</p>

<p>Another issue, from what I&rsquo;ve read, is that the tokens that you get from Google are only good for an hour. Google expects you to re-request tokens as you need them, and provides a way to do those re-requests without bugging the user. Unfortunately, I didn&rsquo;t need to provide my OAuth key/secret to the Android app, which means that the platform is handling that. Initially, I was hoping that I could get the token on the phone and push it up to the server. That way, I could solve the difficult OAuth problem, and do just the Mirror API stuff on the server with the token that I already have. But, if the token expires every hour, I would need to be able to re-request for that token somehow, and I can&rsquo;t do that from the server because the platform is handling that, instead of my app, which means that I would not be able to re-request a token from the server. (I think.) There are probably ways to work around this, but I would assume that they all involve lots of talking between your phone and the server - even if you could use that token on the server.</p>

<p>That said, if all you want to do is push data from the phone to Glass, and don&rsquo;t need to get data back from Glass, this <em>might</em> work OK for you. Keep reading to see how.</p>

<h2>How to</h2>

<p>I&rsquo;ll break this down into two main sections, the Authentication piece, and the Mirror piece. Pretty self explanatory.</p>

<h3>Authentication</h3>

<p>First off, I grabbed a lot of this code from the following blog post: <a href="http://blog.tomtasche.at/2013/05/google-oauth-on-android-using.html" target="_blank">Google OAuth on Android using AccountManager</a>, which was incredibly helpful in getting started with this. So, go read that first, I&rsquo;ll be here when you get back.</p>

<p>Alright, now that you&rsquo;ve got an idea of how to use Android&rsquo;s <code>AccountManager</code>, there are a few tweaks and notes that need to be made to that post. Most importantly, is an error in that post that I mentioned in the comments. When grabbing the user&rsquo;s account, you need to make sure to filter on &ldquo;com.google&rdquo;, before I added this, I kept getting Dropbox: <code>accountManager.getAccountsByType("com.google")</code></p>

<script src="https://gist.github.com/emil10001/6262250.js?file=requestToken.java"></script><p>The <code>SCOPE</code> also needs to be changed to request permissions for the Mirror API:</p>

<script src="https://gist.github.com/emil10001/6262250.js?file=scope.java"></script><p>Those are really the only changes to Thomas&rsquo;s code that need to be made to get the authentication piece working.</p>

<h3>Mirror</h3>

<p>Now that you&rsquo;re authenticating with the AccountManager, we need to plug in some Mirror calls. I think that I&rsquo;ve found a bug here as well, but I&rsquo;ll need to ping the team to make sure. Basically, when you&rsquo;re building the Mirror object, you don&rsquo;t pass it any credentials, instead, the token gets added to the timeline insert request.</p>

<script src="https://gist.github.com/emil10001/6262250.js?file=MirrorService.java"></script><blockquote>
  <p>See how that third parameter in the <code>Builder</code> is null?</p>
</blockquote>

<p>Now, you can take a look at what we can do once we have our token:</p>

<script src="https://gist.github.com/emil10001/6262250.js?file=doCoolAuthenticatedStuff.java"></script><blockquote>
  <p>See the call to <code>.setOauthToken(authPreferences.getToken())</code>?</p>
</blockquote>

<p>If you have Glass and you&rsquo;re running this, at this point you should see &ldquo;Hello from android&rdquo; inserted into your timeline. It did work for me when I was testing it last night.</p>

<h2>Conclusion</h2>

<p>While I&rsquo;m very interested in seeing if I can push this to make a work-able solution, I&rsquo;m concerned that this will be a dead-end, at least for now, given the issues that I outlined above. However, I am certainly open to hearing ideas about how this might be expanded into something that is actually useful, or a way that I could actually use the token server-side. (Obviously, nothing that Google would frown upon.)</p>

<p><a href="https://github.com/emil10001/GlassFromAndroid" target="_blank">Here&rsquo;s the GitHub repo.</a></p>

<h1>EDIT</h1>

<h3>1</h3>

<p>Forgot a couple of things, first, there are a few jars that you&rsquo;ll need to pull in, <a href="https://github.com/emil10001/GlassFromAndroid/tree/master/libs" target="_blank">check out the libs dir of the project</a>. I&rsquo;ll probably need to get this formatted to have maven suck those in auto-magically, but I didn&rsquo;t have much time to get the code working and write this post, so hopefully soon. Second, I had an issue with getting AndroidHttp.newCompatibleTransport working, so I stuck with NetHttpTransport.</p>

<h3>2</h3>

<p>Looks like someone else had done this a couple months back and actually spent enough time to <a href="https://github.com/twaddington/mirror-quickstart-android" target="_blank">give it some polish</a>. For those wondering, I don&rsquo;t really bother with polishing experiments much. They might grow eventually, or they might just stay as they are. The idea is to just test something out quickly, and maybe write about it. I don&rsquo;t have  time for much more than that.</p>

<h3>3</h3>

<p>I just ran into the error: &ldquo;Daily limit for unauthenticated use exceeded. Continued use requires signup.&rdquo; Luckily, I figured out what the issue was, at least for me. I switched to Intellij, and one thing that I didn&rsquo;t realize was that gradle was not grabbing my regular <code>debug.keystore</code> from <code>~/.android/debug.keystore</code>. So, I tried specifying a particular keystore with gradle:</p>

<script src="https://gist.github.com/emil10001/8635874.js?file=android_build.gradle"></script><p>Then just copy the keystore into your module directory:</p>

<script src="https://gist.github.com/emil10001/8635874.js?file=copy_keystore.sh"></script><p>Now, you should be good to rebuild and have things work!</p>
