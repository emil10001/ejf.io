{
    "slug": "bad-peerguardian",
    "date": "2013-06-17T03:14:47.000Z",
    "tags": [],
    "title": "Bad PeerGuardian",
    "publishdate": "2013-06-17T03:14:47.000Z"
}Bad PeerGuardian
================




<blockquote>
  <p>Originally submitted by emil10001 on Sat, 10/01/2011 - 16:10</p>
</blockquote>

<p>I will try to make this one brief. If you are on a mac, do not install <a href="http://en.wikipedia.org/wiki/PeerGuardian" target="_blank">PeerGuardian</a>. This application gave me weeks of trouble for something that I had only used once or twice. Basically, PeerGuardian is a safety tool for using P2P. The idea is that it will blacklist certain IPs/domains based on whether the IP address might be dangerous. Generally, the blacklisted IPs belong to RIAA/MPAA associates, and while I wasn&rsquo;t downloading any movies or music, I figured that I could use some protection if I&rsquo;m going to be using P2P for anything.</p>

<h2>The Issue</h2>

<p>The problem with PeerGuardian for Mac is that it actually did more than just block traffic to certain unsavory IPs while the application was running. Instead, it blacklisted my server, but only for port 22 (possibly other untested ports as well), while leaving port 80 alone. PeerGuardian also blocked access to one of my client&rsquo;s servers. I was able to ssh from my macbook to other machines, and from other machines on my local network to my EC2 boxes.</p>

<h2>Explanation</h2>

<p>(Disclaimer - This section is basically my guessing about factors contributing to my issue. I have no real way of verifying whether or not this is an accurate explanation.)</p>

<p>It seems possible that PG was liberally blocking traffic on certain ports to certain IP groups. If there were some legitimate IPs residing on the EC2 cloud, worthy of blocking, where my VPS is hosted, as well as my client&rsquo;s, that gets us part-way there to an explanation. Again, traffic was flowing through port 80 to both boxes, so PG may have been blocking only certain ports.</p>

<p>The other issue, the thing that made this difficult to track down, was that I had not been running PG when I was experiencing these issues. In fact, since I had downloaded it several weeks ago, I think that I only used it twice. The implication here is that the blacklist exists all the time, and blocks traffic whether you are running PG or not. I did not see any processes running that indicated to me that PG was running in the background, and PG does not use the system&rsquo;s hosts.deny file to define what traffic to blacklist. Now, it was probably not that smart of me to download and run something like this without really looking into how it worked, but how was I supposed to guess that the thing would block traffic whether it was running or not?</p>

<p>These factors contributed to making this a very difficult problem to diagnose, the fact that I didn&rsquo;t use it much, it wasn&rsquo;t running, and it was randomly blocking legitimate traffic to EC2 servers, while allowing other ssh traffic to other places.</p>

<h2>Resolution</h2>

<p>I spent about two weeks, in the few free hours I could find, trying to fix this myself before posting the question on <a href="https://forums.aws.amazon.com/message.jspa?messageID=280885#280885" target="_blank">EC2&rsquo;s forums</a> and ServerFault. My <a href="http://serverfault.com/questions/315382/ssh-connection-refused-only-from-my-mac-my-linux-box-connects-without-issue" target="_blank">ServerFault</a> question provides a detailed look at what was involved in tracking down the root cause of this problem. Basically, once someone suggested that I try a traceroute from my macbook, the failure gave me something new to google, leading me to people with similar issues. The other symptoms that I was seeing were common symptoms of other problems, thus leading to many dead ends.</p>

<p>My advice is to find a different tool for security while using P2P. PeerGuardian can cause more harm than good.</p>

<p>Original discussion on <a href="http://www.reddit.com/r/mac/comments/kxryb/peerguardian_broke_my_mac/" target="_blank">Reddit</a>.</p>
