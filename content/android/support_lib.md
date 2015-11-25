+++
date = "2015-11-23T14:17:11-08:00"
draft = true
title = "support_lib"

+++

<iframe width="560" height="315" src="https://www.youtube.com/embed/ihQ16K8gSuQ" frameborder="0" allowfullscreen></iframe>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Support library by <a href="https://twitter.com/chrisbanes">@chrisbanes</a> and <a href="https://twitter.com/adamwp">@adamwp</a> <a href="https://twitter.com/hashtag/AndroidDevSummit?src=hash">#AndroidDevSummit</a> <a href="https://twitter.com/hashtag/Sketchnotes?src=hash">#Sketchnotes</a> <a href="https://t.co/l3LurcYMeA">pic.twitter.com/l3LurcYMeA</a></p>&mdash; Chiu-Ki Chan (@chiuki) <a href="https://twitter.com/chiuki/status/668926497890045952">November 23, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

* What?
* Why?
* Gotchas?
* How?
* Bugs?

## What?

Lots of stuff. Started out simple, but it's grown. There are multiple support libs, providing different utilities. They're a bridge for getting newer functionality on older API level devices.

The core v4/13:

Animation

    ViewCompat.animate()

Fragments

    FragmentActivity.getSupportFragmentManager()

Features

    <android.supportv4.ViewPager> // or something

Infrastructure

* RecyclerView
* appcompat - large chunks of the UI toolkit

Higher Level:

* mediarouter - chromecast
* design
* preferences
* leanback - TV

## Why?

Unbundled releases:

* Not tied to platform releases
* Bug fixes
* New features

### RecyclerView

* Component providing data-set windowing
* Improves upon an existing framework (?)
* Backports to v7

Provides:

* Animation
* Pluggable
* Enforcing

### AppCompat

* UI compatibility layer for v7+
* Backport framework features, but no more
* Goal: stay up to date with framework

Provides:

* Themes
* Toolbar
* Tinting

### Design

* UI feature lib
* Provides features not in the framework
* Goal: implement high-level UI components

Provides:

* FloatingActionButton - FAB
* NavigationView - NV
* Snackbar (like Toast with an action)
* TabLayout - TL

### Percent

* UI feature lib
* Provides percent based layouts
* FrameLayout and RelativeLayout

Allows you to define layout elements as percentages of the parent.

## Gotchas

Library major version number is the minimum compile sdk version.

E.g. lib v23.1.0 needs compileSdkVersion 23.

Dex limit, all of the compat libs add up to a third of the dex size. Using proguard: `minifyEnabled true`, you can cut it down to something more manageable. With new AS tools, no need to re-run proguard on every iteration.

## How?

Using a variable can help to keep all the support dependencies at the same, correct, version in the `build.gradle` file.

    ext {
        supportLibVersion = "23.1.1"
    }

    dependencies {
        compile "com.android.support:appcompat-v7:${supportLibVersion}"
        compile "com.android.support:design-v7:${supportLibVersion}"
    }

## Bugs

http://b.android.com
