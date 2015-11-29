+++
date = "2015-12-01T06:25:30-08:00"
draft = true
title = "Feedback Q&A (Android Intents Part 6)"

+++

This is the sixth post in a series about sharing information between apps. ([Series Table of Contents](/android/intents/toc).) In this post, I'm going to discuss some of the feedback that I've received so far on this topic. No code this time, but it's an important part of the discussion, and is going to set the stage for the next few installments.

<iframe width="560" height="315" src="https://www.youtube.com/embed/5zEHB4KhhEY" frameborder="0" allowfullscreen></iframe>

## Series Table of Contents

[Follow the series on the Table of Contents page](/android/intents/toc).

## Feedback

Recently, I gave a talk at Silicon Valley's GDG DevFest on this series. The talk went fairly well, and I received a good amount of feedback, which was exactly what I was looking for out of the talk.

[Chiu-Ki Chan](https://twitter.com/chiuki) (Android GDE) took really great notes during the session, here's what she had:

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Building Public APIs on the Phone with intents by <a href="https://twitter.com/emil10001">@emil10001</a> <a href="https://twitter.com/hashtag/gdgsv?src=hash">#gdgsv</a> <a href="https://twitter.com/hashtag/devfest?src=hash">#devfest</a> <a href="https://twitter.com/hashtag/Sketchnotes?src=hash">#Sketchnotes</a> <a href="https://t.co/Bkd8DuGBEU">https://t.co/Bkd8DuGBEU</a> <a href="https://t.co/jUMSZaf4dO">pic.twitter.com/jUMSZaf4dO</a></p>&mdash; Chiu-Ki Chan (@chiuki) <a href="https://twitter.com/chiuki/status/668123096952344576">November 21, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

> By the way, I've been using Chiu-Ki's notes in several of my recent blog posts because they're really helpful. Make sure to follow her on Twitter or G+!

There were a number of great questions that I didn't have great answers for. I thought that it would make sense to post them, and think about where to go from here. I'll also try to categorize them, to make them easier to read together.

### Security

**Q: Rooted phones allow for apps to snoop on the Intents being passed around. How do we secure this?**

There were a few Android GDEs in the room that chimed in and helped answer some of these questions. They pointed out, for example that on rooted phones, you really have no expectation of security.

**Q: Locking down Intent filters to just one app?**

[Ryan Harter](http://ryanharter.com/) mentioned that you can lock down intent filters using signature checking, so that only apps signed with your signature are allowed to send and consume them. However, for that case, it's not really a public API.

**Q: How do I know that no other app is going to intercept an Intent? I.e. if another app starts listening for the exact same Intent, and the information is sensitive, that could be an issue.**

I do think that this question deserves a better explanation. I have 'security' and 'a deep dive on Intents' on my roadmap for this series. So, my current answer is that I'll try to cover those issues in a future post.

### Scalability

**Q: Wouldn't it be more scalable to do this on the backend? In terms of sharing data with multiple platforms. If iOS needs the same integrations, and does them on the backend, providing a second set of APIs is lots more work.**

As far as the scalability question goes, my immediate answer was that the authentication may be able to be done locally. I'm pretty sure that there are accounts APIs for this, and it may be necessary to open an additional in-app API for getting full auth details for the backend web requests. The point is that on-device public APIs may not be the answer to *all* inter-app communications issues, but they should at least be part of the conversation.

Android GDE [Mike Wolfson](https://plus.google.com/+MichaelWolfson) also mentioned that using a local public APIs can really help with reducing network usage.

* Good for offline cases
* Emerging markets care a lot about this
* Better for battery

### Logistical Issues

**Q: In the case where lots of developers open up APIs, how do other apps discover them?**

The question around service discovery and consistent behavior may be able to be addressed by some sort of central repository of APIs. Such a repository may contain highly structured data around how to make requests to any given API, which apps provide such functionality.

**Q: What happens when multiple apps provide the same Intent API, but don't work the same?**

This is why we chose Aviary to use with [Talkray](http://talkray.com), over going with whatever was already on the device. Such a repository/project should also provide some sort of automated testing to attempt to validate that the functionality works consistently between apps claiming to provide similar features.

I have a feeling that once I finish laying the groundwork (and assuming that I'm not completely bored with this topic), that this is where I'll be spending a lot of my time.

### Monetization

**Q: How to monetize?**

I think that the monetization question was interesting, and I'll certainly cover that in a future post in this series. I don't think that it needs to be difficult, but I do think that it requires some thought around doing it in a way that maximizes benefit to you, your users, and your partners (those consuming your API).

### Other thoughts

Ryan also brought up the point that the web paradigm might be a good metaphor for communicating the general idea:

* Intents are like HTTP requests
* Content providers
  * like building a REST interface
* Accounts
  * maybe similar to OAuth

## Conclusion

I'll be circling back to what's been brought up here in future posts. Stay tuned!
