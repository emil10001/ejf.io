{
    "slug": "building-a-docker-image-from-scratch",
    "date": "2015-07-29T13:28:15.000Z",
    "tags": [
        "docker"
    ],
    "title": "Building a docker image from scratch",
    "publishdate": "2015-07-29T13:28:15.000Z"
}Building a docker image from scratch
====================================




<p><img src="https://s3.amazonaws.com/ejf3-public/hosted_files/rr/docker_test/homepage-docker-logo.png" alt="Docker logo"/></p>

<p>This post assumes that you have a working docker environment already set up.</p>

<p>Here are the really simple instructions:</p>

<ol><li>create a Dockerfile</li>
<li>open the Dockerfile</li>
<li>add a FROM statement with a <a href="https://docs.docker.com/userguide/dockerimages/" target="_blank">base image</a></li>
<li>add a RUN statement for running whatever setup commands are needed</li>
<li>add any additional RUN statements necessary</li>
<li>add a COPY statement for copying stuff over from the local filesystem</li>
<li>run <code>docker build -t user/image_name:v1 .</code></li>
<li>run <code>docker run user/image_name:v1</code></li>
</ol><h3>Dockerfile</h3>

<script src="https://gist.github.com/emil10001/0b6f3e02ee9f25c6d689.js"></script><h3>testfile</h3>

<p>The following is the contents of <code>testfile</code>:</p>

<pre><code>this is a test file
</code></pre>

<p>As you&rsquo;re writing your Dockerfile, it may be helpful to run the commands against a real
image as you go, so that you can more easily predict what&rsquo;s happening. Here is how to do
that:</p>

<pre><code>docker pull debian:latest
docker run -t -i debian:latest /bin/bash
</code></pre>

<p>If you want to generate a docker image based off of what you did in the <a href="https://docs.docker.com/userguide/dockerimages/#updating-and-committing-an-image" target="_blank">interactive session</a>
vs a Dockerfile, you can take note of the image id in the console after root. E.g.
<code>root@28934273 #</code> specifies the user is root and the image id is 28934273. Then, you can 
run whatever commands you&rsquo;d like, exit the session, and run the following:</p>

<pre><code>docker commit -m "ran some commands" -a "My Name" \
28934273 user/debian:v1
</code></pre>

<ul><li>-m is the commit message</li>
<li>-a is the author&rsquo;s name</li>
</ul><p>Now, you can run that image:</p>

<pre><code>docker run -t -i user/debian:v1
</code></pre>

<p>You can use the image you just created as a base image in another of your Dockerfiles, so that
you can interactively set up your image initially, and then in the second step, add any <code>CMD</code>
statements to actually run your software.</p>

<h2>Refernces</h2>

<p>Documentation pulled from:</p>

<ul><li><a href="https://docs.docker.com/articles/baseimages/" target="_blank">https://docs.docker.com/articles/baseimages/</a></li>
<li><a href="https://docs.docker.com/userguide/dockerimages/" target="_blank">https://docs.docker.com/userguide/dockerimages/</a></li>
<li><a href="https://docs.docker.com/articles/dockerfile_best-practices/" target="_blank">https://docs.docker.com/articles/dockerfile_best-practices/</a></li>
</ul><h2>Code</h2>

<p><a href="https://github.com/emil10001/docker_new_image_test" target="_blank">Clone from GitHub</a></p>
