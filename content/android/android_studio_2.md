+++
date = "2015-11-24T09:01:24-08:00"
draft = true
title = "Android Studio 2.0 & Android Emulator 2"

+++

<iframe width="560" height="315" src="https://www.youtube.com/embed/fs0eira2pRY" frameborder="0" allowfullscreen></iframe>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">What&#39;s in New Android Studio by Jamal Eason &amp; <a href="https://twitter.com/droidxav">@droidxav</a> &amp; <a href="https://twitter.com/tornorbye">@tornorbye</a> <a href="https://twitter.com/hashtag/AndroidDevSummit?src=hash">#AndroidDevSummit</a> <a href="https://twitter.com/hashtag/Sketchnotes?src=hash">#Sketchnotes</a> <a href="https://t.co/4Bo1Ma9XpB">pic.twitter.com/4Bo1Ma9XpB</a></p>&mdash; Chiu-Ki Chan (@chiuki) <a href="https://twitter.com/chiuki/status/669211643771879424">November 24, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Recap

Launched at I/O 2015:

* GPU Monitor
* Network Monitor
* App Templates
* Theme Editor
* Vector Asset Studio
* Lots of other stuff.

Bottlenecks

* dx
* proguard
* appcompat
* Legacy ...
* ...?

Improvements

### dx

* Improved dx merger
  * *Build Tools 23.0.2+*.
* Run dx in process
  * `dexOptions { dexInProcess = true }`
  * Gradle 2.4+
  * Plugin 2.0.0+
  * Build Tools 23.0.2+
* `org.gradle.jvmargs=-Xmx4096m` to change allocated memory for gradle.

### ProGuard

Issues

* not incremental
* no pre-dexing

Improvements:

* Some incremental support
* does not disable pre-dexing

Currently supports shrinking only
Configured per-variant: Use ProGuard for release (obfuscation, optimizations)

Will allow single pass legacy multi-dex

### App Deployment

Improved deployment speed...?

Built specifically for the connected device during debug builds. (AS only)

Only package one set of assets (e.g. mdpi only)

Later:

* ABIs
* Multi-Dex

Install time for IO Schedule (5MB) on N6  was 12 seconds.

**Instant Run**

* Deploy deltas only
* avoid installation
* no app killing

Scenarios:

* Hot swap
* Warm swap
* Cold swap
* Rebuild & Reinstall

Gradle behaves differently when run from Studio
INSTANT_DEV
...?

App Running with Server

* IDE Checks
* Studio -> App connection
* Custom Gradle run
* Optimization: Studio monitors file changes

### Hot swap: Resources

* Send full resources (for now)
* reflection hacks to make it work on the fly
* incremental aapt on the way

Limitations

* manifest changes detected by Studio, triggers full build
* any ID changes, including values trigger a full/new build

## Cold Swap

* incompatible changes
* ...?

## Instant Run and Build

Here's a quick overview of what a build looks like with Instant Run turned on:

<img alt="clean build" width="75%" src="https://storage.googleapis.com/ejf-io/android_dev_summit/clean_build.jpg">

Simple UI

* <img> Run
* <img> Instant Run
* <img> Debug
* <img> Instant Debug

* Just press run
* also have a stop button
* optionally restart activity

Need to turn things on in the Instant Run prefs.

Data binding is broken in the preview build, working in nightlies.

## Emulator

*Performance* - Faster deployment of apks

### Features

New UI for lots of tools

* Mock GPS locations
* Mock different cellular network types
* Fingerprint
* Rotate
* Resize
