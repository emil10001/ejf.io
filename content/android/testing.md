+++
date = "2015-12-08T09:11:57-08:00"
title = "Testing in Android"

+++

*On November 23 & 24, I attended the first ever [Android Dev Summit](https://androiddevsummit.withgoogle.com/). The following are notes that I took during talks. I have included the video of the talk as well. Again, these are my notes from the talk, not my original content.*

<iframe width="560" height="315" src="https://www.youtube.com/embed/vdasFFfXKOY" frameborder="0" allowfullscreen></iframe>

Chiu-Ki Chan (Android GDE) took really great notes during the session as well, here's what she had:

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Android Testing with <a href="https://twitter.com/onlythoughtwork">@onlythoughtwork</a> <a href="https://twitter.com/ppvi">@ppvi</a> <a href="https://twitter.com/jfschmakeit">@jfschmakeit</a> <a href="https://twitter.com/hashtag/AndroidDevSummit?src=hash">#AndroidDevSummit</a> <a href="https://twitter.com/hashtag/Sketchnotes?src=hash">#Sketchnotes</a> <a href="https://t.co/w30eLkXZvm">pic.twitter.com/w30eLkXZvm</a></p>&mdash; Chiu-Ki Chan (@chiuki) <a href="https://twitter.com/chiuki/status/669265757457354752">November 24, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

People want automated testing. Android has not provided a ton of testing APIs, and it hasn't evolved much since API level 3. Until recently.

Android provided tools for testing:

* Android Studio / Gradle
* Android Testing Support Library
* Espresso

Refactor across unit and instrumentation tests.

## Hermetic testing

<img alt="isolate tests from external dependencies" width="75%" src="https://storage.googleapis.com/ejf-io/android_dev_summit/testing_isolate.jpg">

Flaky tests are worse than no tests. (When it fails sometimes.)

Steps:

1. Isolate from external dependencies
  * Especially the network
1. Replace the components that talk to external dependencies, with things that replace with fake data
1. Using product flavor 'mockDebug', with source sets
1. Inject lives in both mock and prod source sets

When to use:

* **Mock mode** for manual testing & concurrent development
* **Prod mode** for end-to-end tests
  * great pre-release check

<img alt="testing pyramid" width="75%" src="https://storage.googleapis.com/ejf-io/android_dev_summit/testing_pyramid.jpg">

## Unit testing

<img alt="unit test business logic" width="75%" src="https://storage.googleapis.com/ejf-io/android_dev_summit/testing_unit_tests.jpg">

Unit tests are fundamental. Run locally, generally fast, should be small and well contained. Focused on methods.

Don't put everything into Activities, it makes it really hard to test. Move business logic out, so that it is more testable.

Mockable Android Jar.

Android Studio helps with Test-Driven-Development. If everything is set up correctly, AS can even help with new features, if tests are defined first.

## UI Testing with Espresso

<img alt="android espresso" width="75%" src="https://storage.googleapis.com/ejf-io/android_dev_summit/testing_espresso.jpg">

Integration and UI tests check your application through its interface. You need a device or emulator for this. CloutTest Lab allows you to test on real devices.

UI testing is hard! Espresso helps with this.

Tried to ask: What would a user do?

**Find, perform, check**

    onView(ViewMatcher)
        .perform(ViewAction)
        .check(ViewAssertion);
