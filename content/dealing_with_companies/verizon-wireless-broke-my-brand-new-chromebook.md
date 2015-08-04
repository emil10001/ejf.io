{
    "slug": "verizon-wireless-broke-my-brand-new-chromebook",
    "date": "2013-04-15T01:07:00.000Z",
    "tags": [
        "verizon",
        "chromebook",
        "pixel"
    ],
    "title": "Verizon Wireless broke my brand new Chromebook Pixel LTE",
    "publishdate": "2013-04-15T01:07:00.000Z"
}Verizon Wireless broke my brand new Chromebook Pixel LTE
========================================================




<p>Wow, looking at my last few blog posts, I&rsquo;ve had a bad few weeks dealing with corporations. After this, I promise to get back to my regularly scheduled programming and not complain so much. Alright, on to today&rsquo;s story.</p>

<h2>Why I&rsquo;m writing about this</h2>

<p>Chromebook Pixel LTE owners should read this if they&rsquo;re planning on activating their SIM, especially if they&rsquo;re adding it to an existing shared data plan, as opposed to simply activating it through the guided tool provided with the device. I don&rsquo;t know exactly how Verizon broke my device, so I&rsquo;d love to hear from Google if they have any thoughts on this.</p>

<h2>My Pixel</h2>

<p>For a few months now, I&rsquo;ve been really wanting to pick up a new laptop. I had my company&rsquo;s laptop, and a few devices at home, but nothing that I could really feel free and comfortable hacking on my own projects on. I had been looking around for a while, and sort of settled on a Macbook Retina 15. I liked the specs, and the great screen, but I wasn&rsquo;t very excited about getting another Apple product. I had also looked at a few PC laptops with the thought of installing Linux on them. However, the best supported among those options was the Dell Sputnik 2, and I was a bit worried it being able to handle a few use cases. I also was a bit worried that if I bought a machine to install Linux on, that I would spend more time playing with Linux than I would developing. Then, Google announced the Pixel, and a few Googlers were posting about crouton. Crouton is a project that allows Chromebooks to run Linux in a chrooted environment, with full hardware support for just about everything that you could think of, including being able to use multiple displays. Perfect.</p>

<p>I decided that a home server would be a good compliment to the Chromebook, for doing the heavier sorts of tasks that I like to run once in a while, like compiling Android from source. And, buying the Pixel and server together was less than the Macbook Retina. I decided to go with the LTE version since I have Verizon, and occasionally will tether my phone to my laptop when I need the web, and I figured that I&rsquo;d prefer to have something on-board rather than killing my phone&rsquo;s battery.</p>

<h2>Setting up the Pixel</h2>

<p>Now, as I mentioned, I wanted to be able to run crouton, so that meant that I needed to enable dev mode on my Chromebook. Enabling dev mode means that you basically wipe the device, and start from scratch, running an unverified instance of Chrome OS. From there, you can set up crouton and install Ubuntu. Now, anyone who&rsquo;s installed some flavor of Linux on a laptop knows that no matter how well supported the device is, there are still going to be pain points, things that will require you to spend some time and solve. The Chromebook Pixel was no different. That&rsquo;s not to say that there were any overly complicated stumbling blocks, but there was a decent amount of setup, couple with my setting up things like Eclipse and ADT for my Android development work. Altogether, I&rsquo;d estimate that I had put about 15-20 hours worth of work into getting my environment set up the way that I wanted, and eliminating the issues that I had initially had with the setup.</p>

<p>Once I had the thing set up, I was very happy with it. The Pixel checked all the boxes of what I needed in a machine, and an environment. The next thing to do was to get the LTE set up, so that when I go off to coffee shops with over-crowded WiFi, that I could still be productive. Off to Verizon!</p>

<p><strong>EDIT: <a href="http://support.google.com/chromeos/bin/answer.py?hl=en&amp;answer=1346275" target="_blank">Instructions on how to do this yourself</a>.</strong></p>

<h2>Meanwhile, at Verizon Wireless</h2>

<p>I made the trip to a local Verizon Wireless store to have my brand new (&lt; 1 week) Chromebook Pixel LTE device added to my shared data plan. My wife and I have a shared data plan, and adding the Pixel was supposed to cost around an extra $10/month, I figured that&rsquo;s not a bad deal. We go in, and the reps are pretty nice, the guy that I was working with hadn&rsquo;t seen the Pixel before, but seemed to be familiar enough with Chromebooks, at least in theory. We got started, and he asked me to hand over the device. No problem, I figured that they were used to doing this sort of thing, what could go wrong, except for maybe losing my browser session.</p>

<p>When he handed it back to me the first time, because he couldn&rsquo;t find the SIM card&rsquo;s IMEI, I noticed that he had killed my browser session. It was annoying, and certainly inconsiderate, but not a huge deal, I was looking up some stuff on Go Lang, nothing I couldn&rsquo;t find again. I didn&rsquo;t think much of it. I found the IMEI, and handed the device back. He seemed to be having trouble with their system, in trying to add the device to my shared data plan. This was a bit annoying, but it&rsquo;s a brand new device and apparently Verizon&rsquo;s process is device-centric. With Verizon, they don&rsquo;t have a general process where they can simply take a SIM card, say, this now belongs to so-and-so&rsquo;s shared plan, and will cost $10/mo to connect. This seems like the way it should be, but it isn&rsquo;t. Instead, they have to find the device in their system, and then see what plans are available for it, and then see if they can hook it up to an existing account. Apparently, the tablet version of their software was allowing them to do this, but the desktop version wasn&rsquo;t. No idea why, I didn&rsquo;t really care either. At this point, the guy called over a manager to help him out. She knew that there had been a rep that had done this before, but apparently nobody in the store had, or knew exactly what to do, so they were going to just muddle through.</p>

<p>Eventually, they were able to get the plan added to the account correctly, and all that was left was to active the SIM. They asked me to reboot the device, hoping that would help. I rebooted it, signed in, and still was unable to connect. I handed it back, and they worked on it for another 5-10 minutes. Then, they seemed to get excited, saying that they had successfully activated it, and that I just had to log back into my account. This time though, when they handed me my device the &lsquo;login screen&rsquo; was not the login screen, instead it was the device setup screen. WTF? I was very confused.</p>

<p>I entered my credentials and was greeted with a blank, fresh screen, just like the one that I saw when I booted the device for the first time. I was very worried at this point. I tried opening a shell, a sure-fire way to test if you&rsquo;re in dev mode or not, I opened the console and entered 'shell&rsquo;, it returned &ldquo;Unknown command: 'shell&rsquo;&rdquo;. Shit. I rebooted the device, no dev-mode scary warning screen. There goes 15 hours worth of work.</p>

<h2>I am livid.</h2>

<p>I calmly looked at the manager and informed her that they had wiped my device in the process of setting up LTE access. She didn&rsquo;t really know what I meant, and I tried my best to explain. I said that everything was gone, not just my account, but everything on the device, things that weren&rsquo;t a part of what gets automatically backed up. She was very apologetic, and I let her know that a simple apology was not going to cut it. She asked me what I wanted her to do, and I said, make me an offer. She went into the back room for about 10 minutes or so.</p>

<p>When she came back, she said that she had spoken with their financial department and determined that they would give me a $35 credit to my account. I believe that that&rsquo;s what they charge for device activations. I calmly explained that I had spent about 15-20 hours configuring the device, and that I would have to do that again. I also explained that if I were to bill out my time hourly, the low end would be at around $100/hr. I said that a $35 credit was not going to cut it. I tried again to get her to understand what she had done. I said, &ldquo;imagine that you had given me your laptop for me to install something on it. I do that, and I hand it back to you and everything is gone. All your music, all your data, everything. How would you feel? Do you think $35 would cover it?&rdquo; Unfortunately, my time had run out, and I needed to leave. I couldn&rsquo;t wait around for another round of her checking with the financial department. I asked her to have them call me tomorrow.</p>

<h2>Why I&rsquo;m so upset</h2>

<p>One thing that really bugged me about the experience was that neither rep took any responsibility for what they had done. They kept saying, &ldquo;all we did was activate the LTE modem, I don&rsquo;t know how this could happen&rdquo;. If you&rsquo;re going to work on someone&rsquo;s device, and you break it, you need to accept responsibility for that. End of story. Another thing that bugs me about this is that while it is part of the Verizon rep&rsquo;s job to activate Chromebook devices, they don&rsquo;t seem to have had any training about it, or even to know the basics of how to navigate the OS. This is a failing on both Verizon and Google. Google went out with an LTE device, with Verizon as a promotional partner, and didn&rsquo;t make sure that the Verizon reps had the proper training to deal with the device without breaking it.</p>

<p>To be fair to the reps, what likely happened is that they rebooted my device, and hit the space bar - that&rsquo;s how you get the device back into verified mode. I had asked them if they rebooted it, and they said that they hadn&rsquo;t; I honestly don&rsquo;t know if they knew enough about what they were doing to be sure. It&rsquo;s also possible that activating the new SIM card really did hard-reset the device, or that part of their instructions were to take it out of dev mode, and they simply didn&rsquo;t own up to that. I don&rsquo;t know exactly what happened, because I couldn&rsquo;t see what they were doing. Regardless, what matters is the result, and that they caused it. I trusted them with my device, and they violated that trust.</p>

<p>Google is owed some of the responsibility here for making it so easy to wipe out a device in the course of doing something seemingly simple, like activating a SIM card. I&rsquo;ll be filing a bug report today on Chrome OS about switching between dev and verified modes, that it should require the primary user&rsquo;s password.</p>

<h2>Where things are now</h2>

<p>There&rsquo;s no resolution at present. I&rsquo;m waiting to hear from Verizon. I would like them to offer me something substantial for the grief that they&rsquo;ve caused. I will also be asking them what they plan on doing in the future to ensure that this sort of thing does not happen again. I will update this post when this issue gets resolved.</p>

<h2>Updates</h2>

<h3>Update 1:</h3>

<p><a href="https://plus.google.com/109919065690021169201/posts/VrnqNzjx186" target="_blank">+Charles Hardy</a> mentioned that if you can talk the VZW reps into giving you the card and doing it yourself, it&rsquo;s a fairly easy process. He said that you just need to get a SIM that&rsquo;s been added to your account, and replace the SIM in the Pixel with the new one. Then, reboot and go through the activation process on the Pixel like normal.</p>

<h3>Update 2:</h3>

<p>Just got off the phone with Verizon, they offered a $55 credit, $20 more than the last offer. I told them that that was unacceptable, but the manager I spoke with was not willing to budge. We&rsquo;ll see what happens next.</p>

<h3>Update 3:</h3>

<p>Submitted as an issue to Google. I went through the process suggested by Google for submitting feature feedback and suggestions.</p>

<pre><code>chrome://help/ -&amp;gt; "Report an Issue"
</code></pre>

<h3>Update 4:</h3>

<p><a href="https://plus.google.com/109919065690021169201/posts/VrnqNzjx186" target="_blank">+Melissa Daniels</a> responded to my G+ post on this, and said that she passed my feedback regarding switching between verified and dev mode to the appropriate team. That&rsquo;s good to hear!</p>

<p>More bad news on the Verizon side, I logged into my account this morning and found that they were charging me twice as much as they said they were going to when I was in the store. They told me $10/mo, and I&rsquo;m looking at my account that says $20/mo. The rep that I&rsquo;m talking to seems nice, hopefully she can get some of this fixed.</p>

<h3>Update 5:</h3>

<p>Verizon offered me a $120 credit, and I&rsquo;m taking that one. Amy the CSR came through!</p>

<p>As far as the monthly access fee goes, $20/mo is what Verizon charges for USB modems and netbooks, and $10/mo is what they charge for tablets. They are counting this as a USB modem or netbook, and won&rsquo;t budge on it. Apparently that&rsquo;s how it&rsquo;s in their system, and the various blogs that reported otherwise, along with the in-store rep that I spoke with, were incorrect.</p>

<p>I think that I&rsquo;ll probably keep the data, since I&rsquo;ll be saving roughly that amount on reducing my Dropbox usage, though I&rsquo;m quite unhappy with Verizon on this. There are several reasons that I&rsquo;m still annoyed, but the main one is that I don&rsquo;t really understand why they have vastly different rates to connect devices to the shared data plan that you&rsquo;re already paying for. Some devices are $5, and some are $20.</p>

<p>I think that this is where I will leave off with this post, since the main issue was resolved, and I don&rsquo;t see any movement on the monthly fee issue.</p>

<h3>Update 6:</h3>

<p>Really, this is the last one. Melissa Daniels is going to follow up with VZW on this issue for me, which I really appreciate. She&rsquo;s really working to make sure that everyone with a Chromebook has a great experience.</p>

<p>The last rep that I spoke with at Verizon said that he would be following up with the local store to make sure that the reps there get up to speed on their training, as well as learn how to communicate things like monthly charges better to customers.</p>

<p>All-round, this was basically the result that I was looking for.</p>

<h3>Update 7:</h3>

<p><a href="http://support.google.com/chromeos/bin/answer.py?hl=en&amp;answer=1346275" target="_blank">Here are instructions on how to do this yourself.</a></p>

<h3>Update 8:</h3>

<p>From <a href="https://plus.google.com/u/0/+JeffJarvis/posts/UZpxa5ZGP4A" target="_blank">Melissa Daniels in this post</a>:</p>

<hr><blockquote>
  <p>Hi all,
  As a heads up, for those of you who are looking to activate, the 
  steps for activation are as follows</p>
  
  <p>For calling or in store:
  Process is as follows, user needs their ICCID (sim card ID), IMEI 
  (device ID) and their existing Verizon account # and login details.<br/>
  Call VZW post paid service at 1-800-256-4646 and press #2</p>
  
  <p>We&rsquo;re actively working to resolve these issues and streamline this 
  process (as well as update our content on our own help center as 
  well as on VZW&rsquo;s) and truly appreciate everyone&rsquo;s patience.</p>
  
  <p>As +Trond Wuellner mentioned, the pricing should only be $10 &ndash; not 
  $20.</p>
  
  <p>Thanks!﻿</p>
</blockquote>

<hr><p>So, that&rsquo;s $10/month to add the Chromebook Pixel LTE to a shared data plan coming from two Google reps (Trond Wuellner is the other, see the thread). Trond mentioned in another comment that they (at Google) were working with a team at Verizon to get this issue taken care of and fix the pricing for everybody. I will update here again once I see this reflected in my bill, or if there are any other developments.</p>

<h3>Update 9:</h3>

<p>I did need to call Verizon to get the bill fixed. However, I was only on the phone for about 10 minutes, and everything&rsquo;s in order, including a refund for the extra charges.</p>
