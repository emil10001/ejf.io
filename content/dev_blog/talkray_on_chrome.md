+++
date = "2015-08-14T10:25:16-07:00"
title = "Talkray On Chrome"

+++

### And other Android apps!

There's a way to pull in Android apps into Chrome, and now have Talkray up and running on my laptop! Here's hoping that Chrome support gets better.﻿

<img alt="Talkray on the desktop" width="75%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/desktop_talkray.png">

There are two versions of the how-to. The first is something that *should* work, but that I have not tested. The second, is what I actually did, though it is more work. What's more, this should work for many different Android apps.

Here's the one that should work:

1. Install the Android version of the [Evernote](https://chrome.google.com/webstore/detail/evernote/dhfolfjkgpeaojbiicgheljefkfbbfkc?hl=en-US) app in Chrome
1. Install [twerk](https://chrome.google.com/webstore/detail/twerk/jhdnjmjhmfihbfjdgmnappnoaehnhiaf) (Android APK packager for Chrome OS)
1. Run twerk from Chrome App Launcher
1. Download Talkray apk from somewhere.
1. Then drag an apk of Talkray into Twerk
1. Set the package name to com.talkray.client, and then set it to be tablet and landscape
1. Save it, and load the output into Chrome as an unpacked extension
1. You may be able to manually edit the generated output to add a different icon

Here's what I actually did:

1. Download this: http://archon-runtime.github.io/
1. Unzip it, load it into Chrome from chrome://extensions as an unpacked extension
1. Install [twerk](https://chrome.google.com/webstore/detail/twerk/jhdnjmjhmfihbfjdgmnappnoaehnhiaf) (Android APK packager for Chrome OS)
1. Run twerk from Chrome App Launcher
1. Download Talkray apk from somewhere.
1. Then drag an apk of Talkray into Twerk
1. Set the package name to com.talkray.client, and then set it to be tablet and landscape
1. Save it, and load the output into Chrome as an unpacked extension
1. You may be able to manually edit the generated output to add a different icon

The major difference is that I manually downloaded and installed the Android runtime. The runtime is supposed to be installed when Evernote is installed, since it's also an Android app, and requires the runtime. However, I think that the version that I'm using is different than the one provided by Google. I have not had time to test the differences between the two.
