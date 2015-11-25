+++
date = "2015-11-25T07:06:28-08:00"
title = "Notification Best Practices"

+++

*On November 23 & 24, I attended the first ever [Android Dev Summit](https://androiddevsummit.withgoogle.com/). The following are notes that I took during talks. I have included the video of the talk as well. Again, these are my notes from the talk, not my original content.*

<iframe width="560" height="315" src="https://www.youtube.com/embed/5bJTSsk4sLU" frameborder="0" allowfullscreen></iframe>

Not what you can do, as a developer, but what are the devices for? They're for people.

### *Respecting User Attention*

* Don't annoy the user
* Respect them
* Empower them
* Delight them
* Connect them to the people that they care about

## API Levels

Discussed in this post will be the following:

* Lollipop
* KitKat
* Jelly Bean
* All

## Don't annoy the user

### *Users can block notifications*

You probably don't want your app to get banned by the user. The system won't tell you that you've been banned. The user can also uninstall you. Don't push the user to this point.

### *Do Not Disturb Mode*

[I personally have this on all the time.](/products/notifications/)

## Notification Categories

As a developer, you can specify what *type* of notification you are. Are you an alarm, a message, or are you annoying garbage? (Sorry, but you know who you are.)

* CATEGORY_ALARM
  * user requested alarms or timers
* CATEGORY_REMINDER
  * user requested reminder
* CATEGORY_EVENT
  * an event on the user's calendar
* CATEGORY_MESSAGE
  * an incoming direct message, not asynchronous messages
  * e.g. SMS, IM, text messages sent directly to the user
* CATEGORY_CALL
  * an incoming voice or video call
* CATEGORY_EMAIL
  * an incoming asynchronous message
  * e.g. email
* CATEGORY_SOCIAL
  * an update from a social network
* CATEGORY_RECOMMENDATION
  * a specific, timely recommendation for a single item

## Respect them

*No BuzzFeed type notifications, "You'll never believe...".*

### Priority

Users are more receptive to notifications that they get a the right time. For low priority notifications, that may be when the user has a few minutes of downtime. Min priority notifications are not shown in the status bar. Messages are maybe more important, users may want those immediately.

(Jelly Bean +)

    Notification.compat.PRIORITY_LOW

* PRIORITY_MAX
  * time-critical tasks
  * incoming calls, turn-by-turn directions
* PRIORITY_HIGH
  * important communications
  * chats, texts, important emails
* PRIORITY_LOW
  * not time sensitive
  * social broadcasts
* PRIORITY_MIN
  * contextual or background information
  * recommendations, weather

Setting the priority:

    new NotificationCompat.Builder(this)
        .setSmallIcon(...)
        .setContentTitle(title)
        .setContentText(text)
        .setPriority(
            NotificationCompat.PRIORITY_LOW)
        .build();

### Fresh notifications

There are situations that you want to kick off a single notification, and alert the user only once. Timely information like weather, or a traffic incident.

    NotificationCompat.Builder builder =
        new NotificationCompat.Builder(this)
            .setOnlyAlertOnce(true)
            .setProgress(100, 100, false);
    mNoMan.notify(ID, builder.build());


### Stale notifications

Are all the notifications from the app useful? Make sure to cancel stale notifications. When you create the notification, you can set a cancellation policy.

### Fullscreen & Peek

These are very disruptive experiences for users. In Lollipop, there's now a 'peek' type of notification that allows apps to alert the user without interrupting them as much. This happens when the user has the screen on, and is interacting with their phone. This only works for `PRIORITY_HIGH` and `PRIORITY_MAX`, and when the notification is making noise or vibrating. Only use this if you mean to annoy the user.

## Empowering the user

Notifications should be actionable.

* The squash is ripe
  * Works well, user can go harvest squash
* Buy power-up
  * Didn't work, user wasn't there, bad

### Notification Preferences

(Lollipop+)

Long-press the notification, click the 'gear', go in and manage app's preferences.

Add intent-filter in manifest to provide your own.

    <activity android:name="SettingsActivity"
              android:label="@string/app_name">
        <intent-filter>
            <action android:name="android.intent.action.MAIN"/>
            <category android:name="android.intent.category.DEFAULT"/>
            <category android:name="android.intent.category.NOTIFICATION_PREFERENCES"/>
        </intent-filter>
    </activity>

Allows for fine-grained control of preferences in your app.

### Actions

(JB+)

Add up to 3 buttons
can be used with any notification. Get things done in the notification.

Uses:

1. Visit a different activity than the click handler (`contentIntent`)
1. Take some action in the background

**Protip:** To show that an action has been taken, set the intent to null and re-`notify()`.


E.g.

* call notification actions
  * answer
  * decline
* image notification actions
  * view
  * share

### Styles

(JB+)

Rich media notifications shade. You can have the photo in the notification. This looks nice, and gives the user lots of info.

### Global Dismissal

(All)

Sync dismissal of notifications across devices. Requires GCM.

Sending the dismiss action over GCM:

    protected void onHandleIntent(Intent i) {
        if (DISMISS.equals(i.getAction())) {
            int id = i.getIntExtra("nid", 0);
            Bundle data = new Bundle();
            data.putInt("nid", nid);
            String msgId = ...;
            gcm.send(SENDER_ID, msgId, data);
        }
    }

Handling the action on another device after the message has been received by GCM:

    public void onMessageReceived(String from, Bundle data) {
        if (from.startsWith("/post/")) {
            int id = data.getInt("nid");
            n.setDeleteIntent(makeDI(id));
            mNoMan.notify(id, n.build());
        } else if (from.startsWith("/cancel/")) {
            int id = data.getInt("nid");
            mNoMan.cancel(id);
        }
    }

<img alt="flow of notification global dismissal" width="75%" src="https://storage.googleapis.com/ejf-io/android_dev_summit/notifications_global_dismissal.jpg">

### Ongoing notifications

Not able to dismiss these.

Disruptive work-flows. Annoying to users. Users may be unpredictable, and may not use the phone in the same way as you. (Trust me, if you do this wrong, users will yell loudly at you.) Use them rarely, if at all.

Use if:

* startForeground()
* incoming call
* prefer snooze-and-repost pattern

Always give the user a way to dismiss:

* pause/stop/cancel
* consider LOW or MIN priority

(Using a HIGH+ priority on ongoing notifications will be bad for users, and they'll get very annoyed.)

## Delight the user

(All)

Sound. Users love ringtones. Allow users to set custom sounds for different notifications. You get this pretty much for free with `RingtonePreference`, which will get you a URI.

## Connect people

People connect primarily through their phones.

### Downtime & Important People

In Priority mode, you can set calls and messages to notify you for custom contacts. (E.g. starred contacts.)

If you give the system enough information, you can make the cut of apps that can notify the user in Priority Mode, for the contacts that they care about.

*Why?*

* User knows who is important
* App knows who the notification is about
* System uses this to rank and filter

*How?*

Annotate notifications with:

* Contact.CONTENT_LOOKUP_API
* tel:
* mailto:
* Don't need Contacts Permission!

Here's how it might look:

    new NotificationCompat.Builder(this)
        .setSmallIcon(...)
        .setContentTitle(title)
        .setContentText(text)
        .addAction(...)
        .addPerson(Uri.fromParts("tel",
            "1 (617) 555-1212", null).toString())
        .build();
