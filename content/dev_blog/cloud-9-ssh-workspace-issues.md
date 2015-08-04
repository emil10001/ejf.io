{
    "slug": "cloud-9-ssh-workspace-issues",
    "date": "2013-05-03T16:44:00.000Z",
    "tags": [
        "cloud9",
        "cloud computing",
        "chromebook",
        "pixel",
        "web development",
        "ide"
    ],
    "title": "Cloud 9 SSH Workspace issues",
    "publishdate": "2013-05-03T16:44:00.000Z"
}Cloud 9 SSH Workspace issues
============================




<p>Recently, I decided that it would be a really cool thing to have access to a nice Cloud IDE that I could play around with. I landed on Cloud9 IDE, using an SSH workspace.</p>

<p>I had a bunch of issues trying to set up an SSH workspace on Cloud9 IDE, mainly due to using nvm (nodejs version manager). I started out getting these really strange hash mismatch errors. With some help from Cloud 9&rsquo;s support and development teams, I was able to get it up and running.</p>

<h3>Cloud IDEs</h3>

<p>I checked out the options and came back with either Cloud9 IDE or Koding. Koding is pretty cool,  there&rsquo;s some really neat and interesting stuff that they&rsquo;re doing, and as a web-app it&rsquo;s very, very responsive, but it&rsquo;s also very much still beta. There are just certain things that you can&rsquo;t do with it at this point. Cloud 9, on the other hand, has been around for several years. I&rsquo;d wanted to use it on my tablet, but due to the text editor, Ace, not working with touch screens, I wasn&rsquo;t able to use it. Now, with my Chromebook Pixel, the time is right to see if I can get some use out of it.</p>

<h3>SSH workspaces in Cloud 9</h3>

<p>One thing that really caught my eye about Cloud 9 is that they give you one free private workspace, and they have the option of using an SSH workspace. This is perfect for me, because I have my own, quite powerful, development server that I could use. It&rsquo;s awesome because I can install whatever runtimes, compilers or environments I want, use C9&rsquo;s nice interface for editing files, and then use the provided terminal for running the project. You can even use tmux in that terminal, though tmux support is not great. (CTRL + b in the terminal puts you into this strange &lsquo;File Revisions&rsquo; mode in the editor. I&rsquo;m a bit concerned that certain shortcuts in the terminal might do unexpected things in the editor.)</p>

<h3>NodeJS and nvm</h3>

<p>Before I had started with trying to get Cloud 9 connected to my machine, I had installed nvm, since there are times when you need to have different versions of nodejs installed, as required by certain projects or certain tools. I figured nvm would be an ideal solution, because it manages all that for you, and you can install whatever version of node you feel like.</p>

<h3>Attempting to build an SSH workspace</h3>

<p>Initially, I when I started out with this, I figured that I could use nvm to have a few different node versions running, without causing a huge headache. There were some problems, where I had to go in and modify the output of nvm to get it to spit out the version number in the proper format for Cloud9 to read it. (I&rsquo;m not going to include that config, because it ended up not helping.)</p>

<h3>First issue - hash mismatch</h3>

<p>I had to modify nvm.sh to print out only the version number when setting the version, so that Cloud9 could properly parse the response. Now, I&rsquo;m on to the next step, of trying to create an ssh workspace, and here&rsquo;s the error that I get:</p>

<script src="https://gist.github.com/emil10001/5510563.js?file=hash_mismatch_error" type="text/javascript"></script><p>I had no idea what this meant, aside from perhaps it checking the node binary or something? Regardless, Google is not being helpful here, so I fired off an email to Cloud9&rsquo;s support team. They came back and suggested not using nvm, at all, and instead using a regular install of nodejs.</p>

<p>He also said the following:</p>

<p>&gt; We don&rsquo;t do any checksums over the node binaries but we do checksum the packets we send <a href="https://github.com/c9/smith/blob/50f034b29301a955f3e2af9842945bf07db93e62/smith.js#L222" target="_blank">over the wire</a>. This means two things:
&gt; 
&gt; 1. The ssh connection was established successfully
&gt; 
&gt; 2. Somehow the communication with the node agent running on your VM get corrupted</p>

<p>I think that nvm&rsquo;s spitting out the 'v0.x.xx&rsquo; all the time was perhaps causing some problems.</p>

<h3>Next issue - bad gateway</h3>

<p>I decided to build node from source, and install it locally, since I still wanted access to nvm. When I did that, I got the following error:</p>

<script src="https://gist.github.com/emil10001/5510563.js?file=bad_gateway_error" type="text/javascript"></script><p>The developer helping me out had suggested using either node v0.6.21 or 0.10.5, and the version that I had built was 0.11-pre. Ok, time to try something different. I belw away the 0.11 that I had built, and modified my bashrc to alias node as the binary provided by nvm:</p>

<script src="https://gist.github.com/emil10001/5510563.js?file=nvm_bashrc.sh" type="text/javascript"></script><p>I also set the path explicitly in Cloud9&rsquo;s workspace setup as the same path:</p>

<script src="https://gist.github.com/emil10001/5510563.js?file=node_path" type="text/javascript"></script><p>This combination did the trick, and I was able to get my workspace set up, and start coding! (Actually, I haven&rsquo;t started being productive yet, I decided to write this post first.)</p>
