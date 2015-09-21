+++
date = "2015-09-21T07:53:37-07:00"
title = "Running NGINX in Docker without caching"

+++

<img alt="docker nginx" width="75%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/docker_nginx.png">

Lately, I've been working on a little web utility that I decided to write in AngularJS. It started out as a couple of JSP pages, and I quickly realized why things like Angular exist. Anyways, if you don't run AngularJS from a server, it'll complain and want you to load internal bits with XHR. Obviously, you don't want to do this. The solution is to fire up a server.

What I really wanted out of a server was something that was incredibly dumb, and didn't require me to restart the server whenever I updated my frontend code. So, while I could've packaged it with my Tomcat app, and run it in my Tomcat Docker image, that would've required lots of killing and starting the Docker image, just for quick little frontend modifications. I figured that I should be able to use a traditional web server on Docker, and found that both Apache and NGINX were available. There was some reason that I didn't choose Apache, but I don't recall what it was, so I went with NGINX.

The quickest thing to do is to pull the hello world example from dockerhub and remap the volume that hosts the HTML content. This actually works fine until you start trying to quickly iterate. What I found was that the default configuration for NGINX (as used by all of the Docker images that I tried) enables caching. So, even though it's serving the right directory, it may be serving a cached version.

At first, I tried logging into a Docker image, and modifying the config, and using that. That did work, but it wasn't a great process. Instead, I decided to build my own image, and pull in my own config file.

## nginx.conf

Here's the default `nginx.conf` file with a couple of minor modifications. Lines 23-24 have been changed to disable caching, and line 25 turns on the autoindex feature (so that you don't need to type in `/index.html` in the browser).

<script src="https://gist.github.com/emil10001/7f2e5449ae4462d47b67.js?file=nginx.conf"></script>

## Dockerfile

The Dockerfile is really simple, it just takes the stock nginx image, and pulls in our new config file.

<script src="https://gist.github.com/emil10001/7f2e5449ae4462d47b67.js?file=Dockerfile"></script>

## Building and Running

Here's a script I wrote to build and run my image. I tend to treat my Docker images as ephemeral, so I don't really care if they get rebuilt frequently and overwrite some previous version.

<script src="https://gist.github.com/emil10001/7f2e5449ae4462d47b67.js?file=build_nginx_docker.sh"></script>

As a note, on line 11, you'll notice that we map a path that you specify as the volume to serve content from. On a Mac, this must be in `/Users`, and won't work elsewhere.

## Conclusion

If you drop all three of those files into one directory, and run something like the following, you should be good to go:

    $ ./build_nginx_docker.sh /Users/me/webstuffs
