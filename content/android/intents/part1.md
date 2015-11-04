+++
date = "2015-11-03T08:49:49-08:00"
draft = true
title = "How to launch an Activity (Android Intents Part 1)"

+++

This is the first post in a series about sharing information between apps. It will start with the basics, and move on to discuss how we might leverage this to do some interesting things. Additionally, I'll be providing videos that cover the text contents of these posts (plus insightful things), so if you want to watch instead of read, check out the video below:

<youtube video here>

## Sharing in Android

In Android, information is shared between apps in few different ways, there's local files stored in the shared storage area of the device, then there are Content Providers, and Intents. Using local files as information sharing seems pretty hacky, and is wide open for attack, so I'm not going to discuss that. Content Providers are interesting, but this series is going to be discussing Intents. If this goes well, maybe I'll circle back and cover Content Providers.

I'm going to note [the official documentation on sharing](http://developer.android.com/training/building-content-sharing.html) right here:

* [File sharing](http://developer.android.com/training/secure-file-sharing/index.html)
* [Content Providers](http://developer.android.com/guide/topics/providers/content-providers.html)
* [Intents](http://developer.android.com/training/sharing/index.html)

Great, now that that's out of the way, we can get started.

## Example One

Let's say that you want to open one `Activity` from another `Activity`, you'd use an intent like the following:

    startActivity(new Intent(this, ActivityTwo.class));

In this example, the 'information' that you're sharing is simply that you'd like ActivityTwo to appear on top of ActivityOne. If you'd like to see a fully working example of this, [check this tag on GitHub](https://github.com/emil10001/AndroidIntentExamples/tree/step1).

That's really it for the first example. You can run it on your device, and should see something like the following:

<img alt="screen 1" width="49%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/intents/activity1.png" > <img alt="screen 2" width="49%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/intents/activity2.png" >

In the above example, the 'LAUNCH INTENT' button runs the `startActivity` code described above to launch the second activity.

## Example Two

Next, let's extend the first example, and add a button to ActivityTwo that points back at ActivityOne. However, instead of just launching ActivityOne, we'll send back some data along with it, that ActivityOne will display. [Here's the tag on GitHub for this state](https://github.com/emil10001/AndroidIntentExamples/tree/step2).

This time, we'll set an extra on the intent:

    Intent intent = new Intent(this, ActivityOne.class);
    intent.putExtra("EXTRA_CONTENT", "saw ActivityTwo");
    startActivity(intent);

ActivityOne will need to check for the intent extra and then do something with it. Here's how it looks now:

<img alt="screen 1" width="32%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/intents/activity1.png" > <img alt="screen 2b" width="32%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/intents/activity2b.png" > <img alt="screen 1b" width="32%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/intents/activity1b.png" >

## Conclusion

That's it for right now, but you saw a couple of basic things here. We're now launching an Activity as well as passing some data along.
