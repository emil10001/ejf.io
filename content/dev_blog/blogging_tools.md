+++
date = "2016-01-18T09:56:38-0800"
title = "Blogging tools - creating Hugo blog posts from Android"
images = ["preview image or delete"]
description = "I wrote an Android app to create the header data for new blog posts, and other workflow improvements"
+++

I am starting this post on my phone. I realized that one of the roadblocks that I have is that the workflow for creating new blog posts from my phone was nonexistent. I had a git client, and a Markdown editor, but Hugo uses special headers, and I also needed to be able to publish on my server.

Here's what I did:

1. Built an Android app to generate the header, and add it to my clipboard.
1. Add a script to my server for easy publishing.
1. Set up JuiceSSH, so that I can log into my server from my phone.

With those couple things, I can now do the end-to-end process of creating and publishing blog posts from my phone. Here's how that goes:

1. In git client, pull from remote
1. Create new file in repo
1. Open file in JotterPad
1. Run my HugoNew app
1. Paste header into new post file
1. Save file, and push to get repo
1. Log into my server and run publish script

It still needs work (no image support yet), since it is a lot of steps, but it is a start.