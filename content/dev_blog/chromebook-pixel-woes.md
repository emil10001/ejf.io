{
    "slug": "chromebook-pixel-woes",
    "date": "2013-08-20T07:57:00.000Z",
    "tags": [
        "chromebook",
        "chromebookpixel",
        "pixel",
        "programming",
        "development",
        "ubuntu",
        "chromeos"
    ],
    "title": "Chromebook Pixel woes",
    "publishdate": "2013-08-20T07:57:00.000Z"
}Chromebook Pixel woes
=====================




<p>I need to take a minute to complain a bit about my Chromebook Pixel. In order to understand where I&rsquo;m coming from a bit, I think that I should explain a bit about the Pixel, and about my impressions prior to buying it. Several months ago, when the Pixel was announced, it was certainly an interesting looking machine. I was in the market for a new laptop at the time that the Pixel was announced, but I was looking mainly at the MacBook Retinas. The Pixel&rsquo;s specs were OK, excellent display, but a mid-range processor, not a lot of RAM, little local storage, but more than sufficient for delivering a great Chrome OS experience. This was not terribly compelling to me, as a developer who spends most of his time building Android apps.</p>

<p>Then, soon after the Pixel was announced, I started seeing posts on Google+ about crouton, a chroot Linux environment that would allow you to switch between Ubuntu and Chrome OS nearly instantly. The project was maintained by Google, and there were several Google developers who were saying good things about it, and about how they had been mainly using the Pixel for development for some time. I&rsquo;ve never been really crazy about Mac OS, I really like Linux a lot, but finding a machine with great Linux support is tough. The Pixel was looking like it might be that mythical machine. That was really all the convincing that I needed, and it was much less than the MacBook that I was looking at, so I took the leap. As far as the specs, I figured that a chrooted Linux might be alright on 4GB - even though I need an IDE sometimes.</p>

<p>Right off the bat, I put the Pixel into dev mode, got crouton up and running and then proceeded to fix as many of the niggling issues that the experience presented as I could tolerate fixing. I&rsquo;ve got plenty of Gists devoted to fixes for crouton on the Pixel, and I have no idea why the crouton setup scripts couldn&rsquo;t have done these things by default. Eventually, I arrived at something that I thought I could live with. Then I tried to use it.</p>

<p>Don&rsquo;t get me wrong, when all I want to do is browse the web, Chrome OS is great, and I love using it. It&rsquo;s perfect, and fast, and is, for the most part, a great experience. It&rsquo;s also got really good battery life, and it&rsquo;s great being able to ride on CalTrain from Santa Clara to SF with LTE on the whole way, getting work done, both directions and get home with around 50% battery.</p>

<p>However, when I want to get some work done, I get presented with a bunch of little painful things that I need to deal with on the crouton side. First are the fonts and icon sizes of everything. You can tweak those a lot in XFCE, which is light on memory usage, but you can&rsquo;t tweak everything. Chrome, itself, has a hard-coded size for its window elements, making the tab labels difficult to read, and the extension icons tough to distinguish - thank science for muscle memory! Worse, is if you want to learn any new interface within crouton, and the icons and assets within the interface are not resize-able, as is the case with Intellij, it makes things really tough. I actually tried switching to Intellij a couple of times before finally getting it, and the biggest issue was that I simply couldn&rsquo;t read the UI. The key bindings are also not set up properly by default, meaning that you either need to map your own, or deal with some strangeness. The touchpad is particularly bad in crouton, as it&rsquo;s over-sensitive to the point where you need to basically set it to disable itself while you&rsquo;re typing, otherwise strange things can happen on the UI. Now, my cursor has given to resetting itself to some random position on the screen as soon as I take my finger off of the touchpad. It is infuriating to the point where I&rsquo;m starting to consider what my options are.</p>

<p>Again, my complaints are mostly with crouton, and I can deal with those things, for the most part - except the touchpad issues, those kind of drive me crazy. There is one complaint that I do have with the Chrome OS side, and that is that I am experiencing a bug on the dev channel where I get a <a href="https://code.google.com/p/chromium/issues/detail?id=269093" target="_blank">blank screen for about 90 seconds after I log in</a>. Initially, the team jumped on this and was very responsive, but the last comment was something to the effect of, &lsquo;it&rsquo;s a dev mode device with crouton installed&rsquo;, and there was an indication that they should probably bump down the priority since it was somehow my fault.</p>

<p>I would say 'fair enough&rsquo; to all of this, except for one thing, this was  billed as a developer&rsquo;s tool. This was the big thing that was given out at Google I/O this year (yes, I have two, and, to be clear, the one that I&rsquo;m having issues with is the one that I paid for), where the keynote was focused on developers, and developer tools, and <a href="https://www.youtube.com/watch?v=9pmPa_KxsAM?t=53m36s" target="_blank">when discussing the Pixel giveaway at I/O, Sundar said the following</a>:</p>

<blockquote>
  <p>The goal behind the Pixel, was literally to design the best laptop out there possible. &hellip; Our goal with the Pixel was to get it in the hands of developers, so that they can build the next generation of web experiences.</p>
</blockquote>

<p>The goal was to design the best laptop, and get developers using that machine. If they just wanted developers to have access to the platform, well, their lower-end Chromebooks were actually doing a better job of that, since they are incredibly inexpensive. No, the goal was to have developers actually use the thing, and, presumably, develop on it. And, they gave it away at their developer conference, to developers, where there were probably as many Android developers as there were web developers. Given that Chrome OS has no other way of doing real development, to me, this is sort of giving implicit support to projects like crouton, and especially saying that Google is standing behind developers. They should know that we&rsquo;re going to put it into dev mode (it&rsquo;s in the freakin&rsquo; name!), and install a usable environment on it. As an aside, yes, I&rsquo;ve tried all of the different web IDEs, and they all suck. Some more than others, but none of them aren&rsquo;t a hindrance to my workflow.</p>

<p>Where does  this leave me with my Chromebook Pixel? Well, I&rsquo;ve got to do something, I&rsquo;ve got too many projects that I want to work on not to have a workable development environment. The current setup isn&rsquo;t working for me, all of those little niggling issues combined together result in a poor experience at best, and quite frustrating at worst. What I&rsquo;m thinking right now, is that I&rsquo;m probably going to try dual-booting Ubuntu and Chrome OS. On the Chrome OS side, I&rsquo;ll probably roll back to the beta channel, and might install a console-only crouton instance. On the Linux side, it will probably be Ubuntu, or maybe Arch. However, I&rsquo;m at the point where I really don&rsquo;t want to waste a ton of time playing with my configuration, so if after a couple weeks it still isn&rsquo;t working out, it&rsquo;s probably going to land on eBay, and I&rsquo;ll be looking for Apple to announce a Haswell MacBook Retina.</p>

<h2>EDIT</h2>

<h3>1</h3>

<p>The status of the blank screen bug that I reported has been updated to 'started&rsquo;. That&rsquo;s somewhat re-assuring. Now I just need to get crouton in shape.</p>

<h3>2</h3>

<p>Also, to be clear, yes, I realize that a lot of my issues stem from using crouton, and are issues with either Ubuntu or crouton. The main reason for this post was to act as a bit of a catharsis, and to see if others were having similar issues, or if people solved it. Another reason was because I&rsquo;m frustrated. Finally, I <strong>do</strong> think that Google should be providing an officially supported development environment on the Pixel - that&rsquo;s local development on the Pixel, not CRD. Instead, Google seems to be pushing the web IDE angle, which I just don&rsquo;t think is production-ready.</p>

<p>But who knows, I&rsquo;ll certainly be trying some different things (including checking out the web IDEs again) over the next few weeks to try to really figure out how to make my workflow sane on the Pixel. If I can&rsquo;t figure it out, I&rsquo;ll have to move on.</p>

<h3>3</h3>

<p>I have noticed that sometimes when I launch crouton when I am resting my hands on the keyboard, where my palms are likely touching the trackpad, is when I have the most issues with the cursor resetting itself. I had this happen this morning, where I logged out, and logged in again with my hands totally off the machine, and it seemed to work. Hopefully that&rsquo;s all I need to do going forward to not see this bug ever, ever again.</p>

<p>Also, someone else commented on my bug report that they were also seeing the blank screen, but they are not in dev mode. Good news for me, in terms of hopefully getting a fix.</p>
