{
    "slug": "android-wear-ux",
    "date": "2014-08-04T23:18:46.000Z",
    "tags": [
        "android wear",
        "android",
        "Android Development"
    ],
    "title": "Android Wear UX",
    "publishdate": "2014-08-04T23:18:46.000Z"
}Android Wear UX
===============




<h1>Android Wear UX</h1>

<p>I saw the news today about WhatsApp releasing a beta version of their app with Android Wear support. It got me thinking about what makes for a good experience, especially on wearables. This is something that I do think about quite a bit, but I haven&rsquo;t written a lot about.</p>

<p>One thing that popped into my head was that my co-worker and I had solved some of the same problems on our app, Talkray, that WhatsApp is trying to address. While I don&rsquo;t have much insight into WhatsApp&rsquo;s design process, I can talk a bit about what we looked at when we were working on Talkray&rsquo;s Android Wear support. <a href="http://www.recursiverobot.com/post/93314173149/android-wear-sdk-ux-and-data-syncing" target="_blank">I actually presented on this last week</a>, <a href="http://youtu.be/jbbKCfCIyFA?t=8m2s" target="_blank">here&rsquo;s a video of that.</a></p>

<h2>Design Principles</h2>

<p>The <a href="https://developer.android.com/design/wear/index.html" target="_blank">Android Wear design documentation</a> provides some <a href="https://developer.android.com/design/wear/principles.html" target="_blank">guidelines on how to create a great experience on Wear</a>:</p>

<ul><li>Focus on not stopping the user and all else will follow (5 second rule)</li>
<li>Design for big gestures</li>
<li>Think about stream cards first</li>
<li>Do one thing, really fast</li>
<li>Design for the corner of the eye</li>
<li>Don’t be a constant shoulder tapper</li>
</ul><h2>Talkray on Android Wear</h2>

<p><img src="https://31.media.tumblr.com/cdde4953a2122b2d8ac7c0a0df40fc78/tumblr_inline_n9t21gJ0Oj1sq0x3a.png" alt=""/></p>

<p>If you haven&rsquo;t heard of us before, Talkray is a calling and messaging app. Talkray has had Android Wear support since Wear launched. There were two basic things that we wanted to be able to do from the watch. First, was to be able to answer incoming calls. The other thing was to be able to quickly respond to incoming messages. We came up with two basic ways of responding to messages, a canned auto reply and a voice reply.</p>

<h2>Considerations</h2>

<p>We took into account a few basic considerations when designing the UX for our wearable app. First and foremost, we wanted to have very, very quick interactions, as short and simple as possible. There&rsquo;s a 5-second rule that shows up in Google&rsquo;s documentation for Android Wear, and that is that if you force the user to interact with you on a watch for more than five seconds, they might as well have pulled out their phone.</p>

<p>There were several other things that we thought about too, like the fact that Wear is going to be a small screen, with limited interaction capabilities. We didn&rsquo;t want to make the user read or think, as much as possible. We also wanted to make it safer to use while driving, since we know that people do text and drive, even though they shouldn&rsquo;t. I realize that it&rsquo;s a bit controversial to say that, but I felt that if we could cut down the interaction enough, we could give people something that would allow them to quickly and easily respond to messages without pulling their attention away from the task at hand.</p>

<h2>Auto Reply</h2>

<iframe width="560" height="315" src="//www.youtube.com/embed/rfnlTXEvDXM" frameborder="0" allowfullscreen></iframe>

<p>The first thing that we give users is a button to send a canned response to a message. This is actually a bit interesting in that it uses Activity Recognition to figure out what you&rsquo;re doing and respond intelligently. E.g., if you&rsquo;re driving, it will say that you&rsquo;re driving and can&rsquo;t talk right now.</p>

<p>We considered giving multiple choices for canned responses, but felt that doing so would require too much interaction, and would defeat the purpose. There&rsquo;s literally only one thing to do here if you want to send a canned response. No thinking, just hit the button and you&rsquo;re done.</p>

<h2>Voice Reply</h2>

<iframe width="560" height="315" src="//www.youtube.com/embed/U5Vj86v_to4" frameborder="0" allowfullscreen></iframe>

<p>For anything beyond the canned response, we figured that people should be able to say what they want. However, after a year of using Glass, I know that when I see the speech to text running, it distracts me from just saying what I want to say, and I start thinking about what I&rsquo;m reading. This is really bad for two reasons, first, it pulls my attention into the watch. Second, it distracts me from delivering the information that I want to get across.</p>

<p>Providing a voice-only message, that records the actual audio and sends that, means that I don&rsquo;t need to think about what the machine thinks that I&rsquo;m saying, the other person should be able to hear it and figure out what I&rsquo;m saying based on the audio.</p>

<p>The one big down-side to this is that the current crop of watches don&rsquo;t have speakers. This means that while we are encouraging people to send audio messages, they won&rsquo;t be able to receive them. While not ideal, we felt that this was acceptable, though there may be room for improvement here.</p>
