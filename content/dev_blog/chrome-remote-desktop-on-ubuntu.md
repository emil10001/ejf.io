{
    "slug": "chrome-remote-desktop-on-ubuntu",
    "date": "2013-04-26T22:31:00.000Z",
    "tags": [
        "chrome",
        "chrome os",
        "linux",
        "ubuntu"
    ],
    "title": "Chrome Remote Desktop on Ubuntu",
    "publishdate": "2013-04-26T22:31:00.000Z"
}Chrome Remote Desktop on Ubuntu
===============================




<p>Well, that was an adventure. I just got Chrome Remote Desktop set up on my home server, running Ubuntu, <a href="https://plus.google.com/100132233764003563318/posts/2fHNvGUHJ3B" target="_blank">following these instructions</a>.</p>

<p>There were a few wrinkles, mainly caused by my wanting to try out a several different desktops, and then not wiping them out properly. Once I had gotten to the point of being able to connect, I was seeing a wallpaper on a giant desktop that I couldn&rsquo;t do anything with. It looked like I was seeing the extension of a login screen or something.</p>

<p>My solution was to uninstall all of the desktops, and as many of their dependencies and child packages as possible (without wiping out chrome-remote-desktop or xvfb), and then re-installed xubuntu-desktop. After that, all that was needed was to create a .xsession file:</p>

<script src="https://gist.github.com/emil10001/5470873.js" type="text/javascript"></script><p>Then, I killed the process running xvfb and re-connected with CRD, and was off to the races! I still have a gigantic desktop, 3840x1600, so if anyone knows how to configure chrome-remote-desktop to launch a smaller desktop, I&rsquo;d be glad to hear it.</p>

<p>EDIT: Found the solution, edit this file: /opt/google/chrome-remote-desktop/chrome-remote-desktop and to reflect the following changes:</p>

<script src="https://gist.github.com/emil10001/5470938.js"></script>
