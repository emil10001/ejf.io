{
    "slug": "using-cardboard-with-unsupported-vr-viewers",
    "date": "2015-06-02T06:21:11.000Z",
    "tags": [],
    "title": "Using Cardboard with unsupported VR viewers",
    "publishdate": "2015-06-02T06:21:11.000Z"
}Using Cardboard with unsupported VR viewers
===========================================




<p><img width="100%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/rr/tango_cardboard/tango.jpg" alt="Google Project Tango"/></p>

<blockquote>
  <p>The Project Tango booth at Google I/O 2015</p>
</blockquote>

<p>On Saturday, I was hanging out with some friends at the <a href="https://www.eventbrite.com/mytickets/427373048/" target="_blank">SFVR Project Tango Hackathon &amp; VR Expo</a>. The Tango team had handed out Tango devices and <a href="https://www.durovis.com/product.html?id=5" target="_blank">Durovis Dive 7</a> VR masks to develop with. I was feeling pretty dead from Google I/O, and the surrounding parties, but <a href="https://plus.google.com/u/0/+EtienneCaron/posts" target="_blank">Etienne Caron</a> and <a href="https://plus.google.com/u/0/+DarioLaverde/posts" target="_blank">Dario Laverde</a> were busy doing their best to build something on the <a href="https://www.google.com/atap/project-tango/" target="_blank">Tango</a> using the Cardboard SDK. They both fired up demos, and immediately found an issue. The screen was not configured correctly for the device, or the viewer.</p>

<p><img width="100%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/rr/tango_cardboard/cardboard.jpg" alt="Google Cardboard"/></p>

<blockquote>
  <p>Giant Cardboard hanging at Google I/O 2015</p>
</blockquote>

<p>It turns out that Cardboard expects a configuration that is typically provided by the manufacturer of the viewer, but the Dive doesn&rsquo;t come with one. Etienne noticed that there was nothing documented in the Cardboard API that related to screen or viewer configs. He and I started to poke at the debugger, trying to figure out if we could find the place that those values get set. We traced it back to a file on the sdcard, but when we looked at it, we realized that it was in serialized protobuf format. My initial thought was to copy the files that read the protobuf and decode the file, but we realized that there was an easier way, the <a href="https://www.google.com/get/cardboard/viewerprofilegenerator/" target="_blank">Cardboard View Profile Generator</a>.</p>

<p>Etienne and I generated config files, and Dario helped us test. Dario was also working on some other Cardboard related issues. Here&rsquo;s what we did:</p>

<ol><li>Visit the <a href="https://www.google.com/get/cardboard/viewerprofilegenerator/" target="_blank">View Profile Generator</a> from Chrome on a computer that&rsquo;s not the tablet you&rsquo;re trying to configure.</li>
<li>On the tablet, in Chrome, visit the shortlink shown on the main page.</li>
<li>On the tablet, if your instance doesn&rsquo;t go full screen all the way (if you can see the nav bars), install <a href="https://play.google.com/store/apps/details?id=com.gmd.immersive" target="_blank">GMD Full Screen Immersive Mode</a> from the Play Store.</li>
<li>Install the phone/tablet in the viewer.</li>
<li>Back on the computer, hit &lsquo;Continue&rsquo;.</li>
<li>Using the tool, you can dynamically configure the view settings. The tablet screen is synced up with the tool, so the changes should appear on the tablet in real time. (It uses Firebase!)
<img width="100%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/rr/tango_cardboard/resize.gif" alt="dynamically resizing screen"/></li>
<li>Follow the instructions on each field, and watch the changes on the screen. You can tweak them until you have something that looks good to you. Here&rsquo;s the <a href="http://google.com/cardboard/cfg?p=CghHREVzUm9jaxIJVGFuZ29EaXZlHRKDQD0lpptEPSoQAABsQgAA8EEAAFxCAABYQlgBNSlcDz06CHsULj4K1yM-UABgAg" target="_blank">config that I generated</a>.</li>
<li>Next, you should be able to generate your profile.
<img width="100%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/rr/tango_cardboard/final.jpg" alt="final screen config"/></li>
<li>In your Cardboard app, you should be able to scan the QR code in the setup step of Cardboard, or go to Settings.</li>
<li>If you&rsquo;re on the Tango, you will need to go through one extra step, the camera that attempts to scan the QR code doesn&rsquo;t work right, so you will need to use a second device.</li>
<li>After scanning from the second device, plug it into a computer with <code>adb</code> installed, and run the following command:
    adb pull /sdcard/Cardboard/current_device_params ./</li>
<li>Then, plug your tablet in, and push the config that you generated:
    adb push current_device_params /sdcard/Cardboard/current_device_params</li>
<li>Fire up the Cardboard app on your tablet and check it out!
<img width="100%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/rr/tango_cardboard/demo.jpg" alt="Cardboard demo"/></li>
<li>If it needs tweaking, just repeat steps 6-12.</li>
</ol><p><a href="https://s3.amazonaws.com/ejf3-public/hosted_files/rr/tango_cardboard/current_device_params" target="_blank">Here&rsquo;s the final config file that I generated.</a></p>
