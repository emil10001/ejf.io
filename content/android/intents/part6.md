+++
date = "2015-11-22T09:25:30-08:00"
draft = true
title = "Comments, Criticisms, and Thoughts (Android Intents Part 6)"

+++

Yesterday, I gave a talk at Silicon Valley's GDG DevFest on this series. The talk went fairly well, and I received a good amount of feedback, which was exactly what I was looking for out of the talk.

There were a number of great questions that I didn't have great answers for. I thought that it would make sense to post them, and think about where to go from here.

* Rooted phones allow for apps to snoop on the Intents being passed around.
* How do I know that no other app is going to intercept an Intent?
  * If another app starts listening for the exact same Intent, and the information is sensitive, that could be an issue.
* Locking down Intent filters to just one app
  * Not really public API
* Wouldn't it be more scalable to do this on the backend?
  * In terms of sharing data with multiple platforms. If iOS needs the same integrations, and does them on the backend, providing a second set of APIs is lots more work.
* What happens when multiple apps provide the same Intent API, but don't work the same?
  * This is why we chose Aviary, over going with whatever was already on the device.
* In the case where lots of developers open up APIs, how do other apps discover them?
* Using a local public APIs can really help with reducing network usage.
  * Good for offline cases
  * Emerging markets care a lot about this
  * Better for battery

There were a few Android GDEs in the room that chimed in and helped answer some of these questions. They pointed out, for example that on rooted phones, you really have no expectation of security. Ryan Harter also mentioned that you can lock down intent filters using signature checking, so that only apps signed with your signature are allowed to send and consume them. However, for that case, it's not really a public API.

The other security question, however, around other apps intercepting sensitive data, I do think deserves a better explanation. I had had security and doing a deep dive on Intents on my roadmap for this series. So, my current answer is that I'll try to cover those issues in a future post.

As far as the scalability question goes, my immediate answer was that the authentication may be able to be done locally. I'm pretty sure that there are accounts APIs for this, and it may be necessary to open an additional in-app API for getting full auth details for the backend web requests. The point is that on-device public APIs may not be the answer to all inter-app communications issues, but they should at least be part of the conversation.

The questions around service discovery and consistent behavior may be able to be addressed by some sort of central repository of APIs. Such a repository may contain highly structured data around how to make requests to any given API, which apps provide such functionality. It should also provide some sort of automated testing to attempt to validate that the functionality works consistently between apps claiming to provide similar features.

Other thoughts:

* Check out intent filters
* Maybe put a key in the metadata of an app that can be inspected from the package manager.
* Talk a little about private APIs

Web metaphor may be useful for communicating the general idea.

* Intents are like HTTP requests
* Content providers
  * Like building a REST interface
* Accounts
  * maybe similar to OAuth
