+++
date = "2016-03-06T10:30:04-08:00"
draft = true
title = "container_tomcat_graph - part 4"

+++

So far in this series, we have [created a new Tomcat app, added it to a Docker container](https://ejf.io/dev_blog/container_tomcat_graph_1), [run that container in Google Cloud with Kubernetes](https://ejf.io/dev_blog/container_tomcat_graph_2), and [added a Titan graph as data store](https://ejf.io/dev_blog/container_tomcat_graph_3). This blog post is going to cover using AngularJS as a frontend.

Here are the technologies that I'm going to use to build this app:

* [Google Container Engine](https://cloud.google.com/container-engine/)
* [Gradle](http://gradle.org/)
* [Docker](https://www.docker.com/)
* [Kubernetes](http://kubernetes.io/)
* [Tomcat](http://tomcat.apache.org/)
* [Titan DB (graph database)](http://thinkaurelius.github.io/titan/)
* [AngularJS 1.x (frontend)](https://angularjs.org/)

As a prerequisite, to this, you'll need to have Docker set up locally, and run through [part 1](https://ejf.io/dev_blog/container_tomcat_graph_1), [part 2](https://ejf.io/dev_blog/container_tomcat_graph_2), and [part 3](https://ejf.io/dev_blog/container_tomcat_graph_3) of this series.

## API

Since we are working off of the same application that we have been building in previous installments of this series, we can use the JSON API [that we built last time](https://ejf.io/dev_blog/container_tomcat_graph_3).

We can issue a request like this:

    GET http://example.com:8080/?name=Thompson,John

And you should receive a response with a structure like the following:

<script src="https://gist.github.com/emil10001/b80b17912d87b5a16795.js?file=name_response.json"></script>

## init Angular

I created a separate module for the web stuff in a directory called 'web', which is outside of the Tomcat app code. I just used the static web generator tool in IntelliJ to generate an Angular module. I would suggest using whatever typical Angular generation tool you're familiar with. IntelliJ uses [the angular-seed project](https://github.com/angular/angular-seed) as its base, so that will be the format that I'll be working with.

There are some good reasons that you may want to keep the web frontend separate from your backend, even hosting the backend on a different subdomain (e.g. frontend at www.example.com and backend at api.example.com). For one thing it would allow you to make cosmetic changes without disrupting the service for everyone. But I think that the primary reason would be performance. There's no reason to put the load of serving static content on Tomcat, when there are plenty of other ways of doing that, that will be more performant.

If you aren't planning on much load, then combining them may be a good idea to simplify your deployments and server strategy, however this project is not what I would call 'simple'.

## NPM Server

As I said, I set up my project using the angular-seed, and if you check the README for that, it tells you to run `npm start`. Do that, and then move on. 

## Dockerfile

Since we said that we were going to host the frontend separately,

## Conclusion

That's it for this series, you started out with nothing, and now have a cloud service using Docker and Kubernetes, running a Graph Database, with an AngularJs frontend. Hope you liked it!
