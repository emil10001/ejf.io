+++
date = "2015-11-25T07:30:24-08:00"
title = "Android Studio 2.0 & Android Emulator 2"

+++

*On November 23 & 24, I attended the first ever [Android Dev Summit](https://androiddevsummit.withgoogle.com/). The following are notes that I took during talks. I have included the video of the talk as well. Again, these are my notes from the talk, not my original content.*

<iframe width="560" height="315" src="https://www.youtube.com/embed/fs0eira2pRY" frameborder="0" allowfullscreen></iframe>

Chiu-Ki Chan (Android GDE) took really great notes during the session as well, here's what she had:

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">What&#39;s in New Android Studio by Jamal Eason &amp; <a href="https://twitter.com/droidxav">@droidxav</a> &amp; <a href="https://twitter.com/tornorbye">@tornorbye</a> <a href="https://twitter.com/hashtag/AndroidDevSummit?src=hash">#AndroidDevSummit</a> <a href="https://twitter.com/hashtag/Sketchnotes?src=hash">#Sketchnotes</a> <a href="https://t.co/4Bo1Ma9XpB">pic.twitter.com/4Bo1Ma9XpB</a></p>&mdash; Chiu-Ki Chan (@chiuki) <a href="https://twitter.com/chiuki/status/669211643771879424">November 24, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

## Recap

Launched at I/O 2015:

* GPU Monitor
* Network Monitor
* App Templates
* Theme Editor
* Vector Asset Studio
* Lots of other stuff.

## Bottlenecks

* dx
* proguard
* aapt
* Legacy multi-dex
* Upload to device
* Installation

## Improvements

### dx

* Improved dx merger
  * *Build Tools 23.0.2+*.
* Run dx in process
  * Gradle 2.4+
  * Plugin 2.0.0+
  * Build Tools 23.0.2+
* New option to configure memory allocated to gradle
  * `org.gradle.jvmargs=-Xmx4096m` to change allocated memory for gradle.
  * long-lasting daemon, instead of short-lived instance
* Run up to 4 dx tasks in parallel
* Use environment variable android.dexerPoolSize to change

Here's how to enable dex in process:

    android {
        dexOptions {
            dexInProcess = true
        }
    }

### ProGuard

Issues

* not incremental
* disables pre-dexing
  * single jar output
  * full re-dexing every time

Improvements:

* Some incremental support
* does not disable pre-dexing

Implementation:

    android {
        buildTypes {
            debug {
                minifyEnabled true
                useProguard false
            }
            release {
                minifyEnabled true
                useProguard true
            }
        }
    }

Caveats:

* Currently supports shrinking only
* Configured per-variant: Use ProGuard for release (obfuscation, optimizations)
* Will allow single pass legacy multi-dex
* Coming in preview 2 (hopefully).

### App Deployment

*Improved deployment speed*

Built specifically for the connected device during debug builds. (Android Studio only!)

Only package one set of assets (e.g. mdpi only)

Currently, only density is supported. Only package one set of assets (e.g. mdpi only).

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

Notes:

* Gradle behaves differently when run from Studio
  * `-Pandroid.optional.compilation=INSTANT_DEV`
* Create Instant Run specific tasks.
* Prepare APK for Instant Run
  * Bytecode instrumentation
  * Server inside app for IDE to talk to

Here's a quick overview of what a build looks like with Instant Run turned on:

<img alt="clean build" width="75%" src="https://storage.googleapis.com/ejf-io/android_dev_summit/clean_build.jpg">

App Running with Server

* IDE Checks
  * Is Gradle 2.0?
  * Do build IDs match?
* Studio -> App connection
  * Is app running?
  * Is activity in foreground?
* Custom Gradle run
  * Build deltas through a custom task
  * Runs a verifier to ensure that we can do HotSwap
  * Tells Studio
* Optimization: Studio monitors file changes
  * Tells gradle to avoid no-op module builds
  * `-Pandroid.optional.compilation=INSTANT_DEV,LOCAL_JAVA_ONLY`

### Hot swap: Resources

* Send full resources (for now)
* reflection hacks to make it work on the fly
* incremental aapt on the way

Limitations

* manifest changes detected by Studio, triggers full build
* any ID changes, including values trigger a full/new build

## Cold Swap

* incompatible changes
  * require restarting the app, and full build/install
  * but should still trigger the delta update
* working on it, demo soon

## Instant Run and Build

<video width="640" height="360" controls>
  <source src="https://storage.googleapis.com/ejf-io/android_dev_summit/instant_run_2.mp4" type="video/mp4">
</video>

Simple UI

* Run
* Instant Run
* Debug
* Instant Debug

Very easy:

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
