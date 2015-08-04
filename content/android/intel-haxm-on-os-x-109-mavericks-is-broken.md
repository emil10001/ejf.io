{
    "slug": "intel-haxm-on-os-x-109-mavericks-is-broken",
    "date": "2013-11-05T19:24:00.000Z",
    "tags": [
        "android",
        "intel",
        "haxm",
        "emulator",
        "osx",
        "mavericks"
    ],
    "title": "Intel HAXM on OS X 10.9 Mavericks is broken",
    "publishdate": "2013-11-05T19:24:00.000Z"
}Intel HAXM on OS X 10.9 Mavericks is broken
===========================================




<p>HAXM is an Intel driver for running a hardware accelerated version of the Android emulator. It works really, really, really well. When it works. However, there appears to be a bug in the latest version of OS X where trying to load a HAXM accelerated Android emulator will cause a kernel panic, and you&rsquo;ll need to kill your Mac.</p>

<p>Here&rsquo;s the link to the thread on Intel&rsquo;s forums: <a href="http://software.intel.com/en-us/comment/1761988" target="_blank">http://software.intel.com/en-us/comment/1761988</a></p>

<p>I&rsquo;ll be keeping a close eye, because, again, the accelerated Android emulator experience is really that good. It&rsquo;s actually seems to be as fast as the iOS emulator, which is impressive.</p>

<h2>Update</h2>

<p>The same thread says they&rsquo;ve pushed a hotfix, and that it should work now!</p>

<h2>Installing HAXM</h2>

<p>For those of you not on OS X 10.9, HAXM is available for Linux, and Windows as well. Here are some instructions:</p>

<p>You&rsquo;ll need to download and install the Intel x86 image in the SDK manager, preferably for 4.3, then you will need to install the driver from Intel:</p>

<ul><li>Instructions: <a href="http://software.intel.com/en-us/articles/installation-instructions-for-intel-hardware-accelerated-execution-manager-mac-os-x" target="_blank">http://software.intel.com/en-us/articles/installation-instructions-for-intel-hardware-accelerated-execution-manager-mac-os-x</a></li>
<li>Driver download: <a href="http://software.intel.com/en-us/articles/intel-hardware-accelerated-execution-manager/" target="_blank">http://software.intel.com/en-us/articles/intel-hardware-accelerated-execution-manager/</a></li>
</ul><p>Then, set up an emulator in the AVD Manager using the x86 image, and match the RAM to whatever you set in the intel driver setup. Also, make sure to use the &lsquo;use host gpu&rsquo; option in the virtual device settings.</p>
