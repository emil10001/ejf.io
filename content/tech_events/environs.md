{
    "slug": "environs",
    "date": "2014-12-26T19:06:40.000Z",
    "tags": [
        "Android Development",
        "android studio",
        "android wear"
    ],
    "title": "Environs",
    "publishdate": "2014-12-26T19:06:40.000Z"
}

<p><img src="https://31.media.tumblr.com/023c869a8f9cde5ac2299ff79706ff7e/tumblr_inline_nh7dqnQ0OC1sq0x3a.png" alt=""/></p>

<h3>Help for running Android code workshops.</h3>

<p><a href="https://googledeveloperexperts.github.io/Environs/" target="_blank">Environs</a> is a VM image that can be downloaded to help run Android based workshops. It is a VirtualBox image built off of Ubuntu, and includes the latest Android SDK and Android Studio.</p>

<h3>Background</h3>

<p>Earlier this year, I ran my first Google Glass workshop. I learned exactly one thing, people will spend most of the time getting their environments set up, and very little time on the content that you want to get through. For my second workshop, I scaled back expectations, and content, and was at least able to get people up and running within the allotted time. This was a pain point, certainly, but I figured that at least I was helping people. Apparently, setting up a development environment was painful for them, and they needed help.</p>

<p><img src="https://31.media.tumblr.com/6a6f397f7ec26ad730fbfc5a31430204/tumblr_inline_nh7e3kYa7F1sq0x3a.jpg" alt=""/></p>

<blockquote>
  <p>GDG Silicon Valley Android Wear Workshop</p>
</blockquote>

<p>I tried again this Fall with an Android Wear workshop, this time it was at a GDG event, with Googlers running the event (there were two of us providing support during the event). While planning the event, we decided that mostly only experienced, and published Android developers would be invited, and we would allow only a handful of open tickets to go out. The results were mostly the same. This time, we did have a split, where about a third of the attendees were off and working on a project, but the rest were asking about setting up their dev environment. To be fair, working with Android Wear generally means using Android Studio, as opposed to Eclipse, which a good number of the attendees were more familiar with.</p>

<p><img src="https://31.media.tumblr.com/dbd3c89536a34af2e4ab5e3e7ed67dd2/tumblr_inline_nh7e42En6L1sq0x3a.jpg" alt=""/></p>

<blockquote>
  <p>GDG Southern Idaho’s DevFest</p>
</blockquote>

<p>The next workshop I ran was for GDG Southern Idaho’s DevFest. When I spoke with their organizer on the phone, he offered to get everyone set up the day before, so that during the event we could actually cover some of the topics. It worked like a charm, and was a great experience for everyone.</p>

<p>This got me thinking about the problem of development environments in the context of code workshops. An initial solution, was to split the room into different tracts. One group, the prepared ones, would get a presentation on new content, and the other would get a standard presentation on how to set up Android Studio. I did notice that for getting Android Studio set up, the same issues pop up over and over again, so doing it with a group in lock-step might actually make sense. I tried this out at a conference, and was able to get through the setup in a reasonable amount of time. However, I didn’t really like it as a solution.</p>

<p><img src="https://31.media.tumblr.com/37607d63905ffa740259b00b059303f1/tumblr_inline_nh7e55SkTq1sq0x3a.jpg" alt=""/></p>

<blockquote>
  <p>GDEs at this year’s GDE summit</p>
</blockquote>

<p>I was having a discussion on this topic with some other GDEs (Google Developer Experts), and somebody mentioned building a VM, and side-stepping the problem altogether. The VM should be able to be passed out on a flash drive, and that way, you just fire it up, and are all set.</p>

<p>I took the VM suggestion and ran with it. I built a VirtualBox VM based off of the latest Ubuntu desktop image, stripped it down a bit, installed the Android SDK, and Android Studio. I also grabbed some samples for Google Glass and Android TV, since those are not included in the official SDK samples. It is my hope that most people will be able to use this as a base for their workshops without too much trouble.</p>

<p>While I did go through a round of feedback from GDEs, this is largely untested. If you have trouble, please <a href="https://github.com/GoogleDeveloperExperts/Environs/issues" target="_blank">create an issue</a> on the <a href="https://github.com/GoogleDeveloperExperts/Environs" target="_blank">GitHub page</a>, and I’ll look into it.</p>

<h3>Instructions</h3>

<ul><li>Download the image from here: <a href="http://goo.gl/W6OQA7" target="_blank">http://goo.gl/W6OQA7</a></li>
<li>Verify the MD5 sum: 935f80992c6d4ec638382f9626a2fe7a</li>
<li>Import the .ova file into VirtualBox 4.3</li>
<li>Fire it up</li>
<li>Credentials are dev/dev (user/pass)</li>
</ul><p>During the event, pass out a flash drive with the VirtualBox installers for every system, as well as Environs.ova. Have participants install VirtualBox and then import the Environs file. (They’ll need about 16GB free disk space.)</p>

<p>Check out the <a href="https://googledeveloperexperts.github.io/Environs/" target="_blank">Environs website for more information</a>.</p>
