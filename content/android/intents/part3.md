+++
date = "2015-11-08T16:05:05-08:00"
draft = true
title = "Make your phone talk (Android Intents Part 3)"

+++

This is the third post in a series about sharing information between apps. ([Part 1 is here](/android/intents/part1), and [part 2 is here](/android/intents/part2).)

<youtube video here>

## Series Table of Contents

* [Part 1 - The basics](/android/intents/part1)
  * Launch Activity (w/Extras)
  * Launch Service
  * Send Broadcast
* [Part 2 - Why?](/android/intents/part2)
  * Why do we want public APIs?
* Part 3 - Make your phone talk
  * Text-to-speech demo
  * Demonstrate public API working with two apps
  * Support generic text share
* Part 4 - coming soon

## Text to Speech Example

Back to looking at code! Yay!

We want to provide to other apps the ability to fire an intent at us, with some text, that will cause us to speak out the text passed in.

This example uses the [text-to-speech API](http://developer.android.com/reference/android/speech/tts/TextToSpeech.html) provided by Android. Since we're discussing intents, and public app APIs, I'm actually going to skip the implementation of this. It doesn't have much to do with the rest of the discussion, and actually, the ability to skip these sorts of implementation details is exactly what we're after. That said, [all of the code is on GitHub](https://github.com/emil10001/AndroidIntentExamples/tree/step5), if you're interested. There isn't a whole lot needed to do this.

Here's what we will look at, providing a public API, and consuming that API.

### Provider

On the provider side, it's very similar to what we have in our [BroadcastReceiver example from Part 1](/android/intents/part1/). We'll start with the snip from the `AndroidManifest.xml`:

    <receiver android:name=".say.TtsReceiver" >
        <intent-filter>
            <action android:name="io.ejf.intentexamples.say" >
            </action>
        </intent-filter>
    </receiver>

This is our `BroadcastReceiver.onReceive` implementation:

    @Override
    public void onReceive(Context context, Intent intent) {
        Intent sIntent = new Intent(context, TtsService.class);

        if (!intent.hasExtra("SAY_TEXT"))
            return;

        sIntent.putExtra("SAY_TEXT", intent.getStringExtra("SAY_TEXT"));
        context.startService(sIntent);
    }

Notice that we're starting a service, as opposed to trying to run the text-to-speech code in the `BroadcastReceiver`. The reason for that is that there are some asynchronous components of Android's text-to-speech implementation, and the `BroadcastReceiver` will die as soon as `onReceive` is finished executing.

In our `Service`, we have something like the following:

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        if (!intent.hasExtra("SAY_TEXT"))
            return;
        if (null == textToSpeech)
            return;

        String sayText = intent.getStringExtra("SAY_TEXT");
        textToSpeech.speak(sayText, TextToSpeech.QUEUE_ADD, null, null);

        return super.onStartCommand(intent, flags, startId);
    }

The above is a slightly modified version of what's in the project. I skipped how the `textToSpeech` object gets instantiated.

### Consumer

All we need to do to consume this API is fire off an intent:

    Intent intent = new Intent();
    intent.setAction("io.ejf.intentexamples.say");
    intent.putExtra("SAY_TEXT", "Some string to speak");
    sendBroadcast(intent);

The above works from apps other than our demo. This is a public API on the phone. In the example code on GitHub, I actually added the above to a separate app, called "SayLauncher":

<img alt="SayLauncher" width="50%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/intents/SayLauncher.png">

[Check out this tag on GitHub](https://github.com/emil10001/AndroidIntentExamples/tree/step5)!

(Quick side-note here, if you want to check out the repository at this step, first check out the repo, then run the following command: `git checkout -b step5 step5`. That will get you a branch at the step1 tag.)

## Sharing from any app

What we're doing is a special case of the public APIs discussion, because there's already a way of doing this that many apps know about, and use. That is generic text sharing. One note here is that text is generally shared to an `Activity`, but that's not a big deal for us, we can simply implement a dummy `Activity`, forward the `Intent`, then kill the `Activity`. Let's take a look.

First, implement the `Activity` in the `AndroidManifest`:

    <activity android:name=".say.TtsDummyActivity" >
        <intent-filter>
            <action android:name="android.intent.action.SEND" />
            <category android:name="android.intent.category.DEFAULT" />
            <data android:mimeType="text/plain" />
        </intent-filter>
    </activity>

Now, in the `TtsDummyActivity`:

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        Intent intent = getIntent();
        String action = intent.getAction();
        String type = intent.getType();

        if (Intent.ACTION_SEND.equals(action) &&  "text/plain".equals(type)) {
            Intent sIntent = new Intent(this, TtsService.class);
            sIntent.putExtra(Constants.SAY_TEXT, intent.getStringExtra(Intent.EXTRA_TEXT));
            startService(sIntent);
        } else {
            Log.w(TAG, "received unknown intent");
        }

        finish();
    }

<img alt="share via screen" width="50%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/intents/share_via.png">

Whenever possible, it's a good idea to use the popular way of providing APIs to other apps. That way, you don't need specific implementations, and you can rely on the fact that lots of apps are going to be using these mechanisms. Sometimes, it might make sense to offer two ways of doing something, like we did in this example. That way, if there is an app that wants to integrate directly, because they want exactly the behavior that you provide, then they can do that.

[Check out this tag on GitHub](https://github.com/emil10001/AndroidIntentExamples/tree/step6)! ( `git checkout -b step6 step6` )

## Conclusion

We got a lot done here today. First, we finally got around to discussing why we're bothering with this whole exercise. We discussed a real-world example of how this sort of thing is helpful to all parties, users, API providers, and API consumers. Then, we went through a concrete example and demonstrated a fun way to both produce and consume a public API on the phone.

Next time, we'll start thinking of some other applications for APIs on the phone, and we'll pick out another example to work through.
