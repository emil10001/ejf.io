+++
date = "2016-03-06T10:30:04-08:00"
title = "Container Tomcat Graph - part 1"

+++

_Note: I wrote this post 3 years ago, and somehow forgot to publish it. It may contain incomplete sentences, and may be complete nonsense, I am not going to re-read it. Proceed at your own risk._

For years, I've had this problem around cooking at home, which is sort of two-fold. First, I really don't like finding recipes and coming up with meals to cook. It's time-consuming, and when I'm digging through recipes, I get indecisive and overwhelmed with the number of choices, wasting hours a week. The other problem I have is that there are meals that I cook that call for me to buy some ingredient that the meal itself doesn't completely use and that I probably won't use again.

I decided to build a tool to help me solve these problems. The tool will allow me to enter in recipes, it will store them in a graph database, and then it will traverse the graph to build a menu for the week for me. If there are recipes that call for uncommon ingredients that are somewhat expensive that won't be fully used, it should try to find other recipes that also use that ingredient, and prioritize surfacing those recipes.

I wanted to write up my process, as I have had a lot of difficulty, just getting the basics off the ground. Especially with how many moving parts there are in this. Hopefully, if I decide to build another app with a similar stack, it'll be a little quicker with this guide.

This blog post is going to take us through the very basics of building a Docker image that runs Tomcat. The next post will cover getting that image up to the cloud. From there, future posts will cover using AngularJS as a frontend, and adding in the Titan graph.

Here are the technologies that I'm going to use to build this app:

* [Google Container Engine](https://cloud.google.com/container-engine/)
* [Gradle](http://gradle.org/)
* [Docker](https://www.docker.com/)
* [Kubernetes](http://kubernetes.io/)
* [Tomcat](http://tomcat.apache.org/)
* [Titan DB (graph database)](http://thinkaurelius.github.io/titan/)
* [AngularJS 1.x (frontend)](https://angularjs.org/)

As a prerequisite, to this, you'll need to have Docker set up locally. I haven't written a thing on this, so you're on your own here. It's not that hard, the installer does most of the work for you. You'll also need to have the Google Cloud tools set up.

## Create the project

You'll need to start with your basic directory structure, with a handful of basic files. [You can find the full source for this blog post here](https://github.com/emil10001/container-tomcat-graph/tree/0.1).  I'll be using the following structure:

<script src="https://gist.github.com/emil10001/f243b4c8efdfd5ad8841.js?file=directory_structure"></script>

### Tomcat

The very first thing that we'll do, is give a rough outline of our Tomcat app in `build.gradle`:

<script src="https://gist.github.com/emil10001/f243b4c8efdfd5ad8841.js?file=build.gradle"></script>

This uses a gradle plugin that helps us out with building for tomcat, as well as grabbing tomcat dependencies.

Now, let's define our `web.xml` file:

<script src="https://gist.github.com/emil10001/f243b4c8efdfd5ad8841.js?file=web.xml"></script>

All we're really doing with that is defining the `jsp` that we'll use as a place-holder. Speaking of, here's the `index.jsp`:

<script src="https://gist.github.com/emil10001/f243b4c8efdfd5ad8841.js?file=index.jsp"></script>

Just a simple 'hello world', so that we can see that things are working.

### Docker

Now that that's done, we need to be able to run that in some sort of environment. [I grabbed a Dockerfile from jeanblanchard/docker-tomcat](https://github.com/jeanblanchard/docker-tomcat), and modified it to copy my app in.

<script src="https://gist.github.com/emil10001/f243b4c8efdfd5ad8841.js?file=Dockerfile"></script>

As you can see, I'm building an entire new Tomcat image every time I build this. I will probably refactor this into two separate Docker images, one for generic Tomcat stuff, and one that depends on the generic Tomcat one, and pulls my war in.

You'll note that if you try to build now, it's going to fail, that's because you need a `tomcat-users.xml` file. Here is the one from the [jeanblanchard/docker-tomcat](https://github.com/jeanblanchard/docker-tomcat) repository:

<script src="https://gist.github.com/emil10001/f243b4c8efdfd5ad8841.js?file=tomcat-users.xml"></script>

### Running Locally

Now that you've got that done, I like to create a script to build and run things locally, just to test it all out. Here's my `buildrun.sh` script:

<script src="https://gist.github.com/emil10001/f243b4c8efdfd5ad8841.js?file=buildrun.sh"></script>

Try running that with:

    ./buildrun.sh

Once again, you'll need to have your environment set up for the above command to work correctly. If it does run correctly, you should be able to visit the app in your browser, at an address that looks something like this: [http://192.168.99.100:8080/container-tomcat-graph-0.1/index.jsp](http://192.168.99.100:8080/container-tomcat-graph-0.1/index.jsp) (the app name might be different if you used something different than I did).

## To be continued

Check out Part 2 (coming soon) for getting your Docker container deployed.
