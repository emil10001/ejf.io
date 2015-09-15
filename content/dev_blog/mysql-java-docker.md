+++
date = "2015-09-15T09:08:29-07:00"
title = "MySQL in Docker with Java Hibernate"

+++

<img alt="docker mysql logos" width="75%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/docker_mysql.png">

Recently, I started working on a new server project at work, and wanted to be able to run a local dev environment with Docker. This has become my normal flow for a couple of server projects because of how easy Docker is to work with, and especially for the fact that I don't need to set up any of the supporting structure on my machine to run the server. I really dislike needing to install things like Tomcat, Apache, or a MySQL server locally on my machine for development. Every time one of those things needs to be installed, I know that it's one more thing on my machine that I need to maintain, and one more thing that could break and cause me to dump hours into fixing. With Docker, I don't need to care about that, I can fire something off with a repeatable, programatic configuration that may be ephemeral, and disappear when I'm done with it.

My work project has two parts, first is the Java part that uses JPA/Hibernate as an ORM layer which talks to a MySQL database. While the Java portion of this was fairly straightforward, the MySQL part was not.

## Outline

* [Source code](https://github.com/emil10001/mysql-java-docker)
* Basic SQL structure
* Java code
* Java Docker scripts
* MySQL Docker scripts
* Tying it together

## SQL Structure

<script src="https://gist.github.com/emil10001/b98ce510330df7089067.js?file=setup.sql"></script>

Here we have a very basic relational structure in MySQL. We have Users, Tags, and UserTags to tie the two together. If you're familiar with SQL, this should be familiar to you.

## Java code

<script src="https://gist.github.com/emil10001/b98ce510330df7089067.js?file=DatabaseInit.java"></script>

I have a bit more Java code that I could share here, but this is probably the most important bit, which is setting up the connection. If you haven't set things up correctly, establishing the connection will be the first thing to fail. If you're interested in seeing the actual JPA/Hibernate entities, [check out the source code](https://github.com/emil10001/mysql-java-docker).

## Java Docker scripts

<script src="https://gist.github.com/emil10001/b98ce510330df7089067.js?file=java_Dockerfile"></script>

The Java Dockerfile is dead simple, basically, we're just copying a jar to a stock java8 Docker image, and running that jar.

## MySQL Docker scripts

<script src="https://gist.github.com/emil10001/b98ce510330df7089067.js?file=sql_Dockerfile"></script>

Next up, is the MySQL Docker file. It's not terrible, but there are a couple of scripts, and something a little non-obvious going on.

When I started poking around with MySQL and Docker, I wanted to use the official MySQL base, as opposed to rolling my own. While I did find a good example run command in the [CoreOS documentation](https://coreos.com/products/enterprise-registry/docs/latest/mysql-container.html), I didn't find one that used a Dockerfile, and a SQL script to set up the database. So, I had to start digging.

One of the first things that I learned was that the `mysqld` command in the `CMD` line does not run the system daemon directly. Instead, it is run through the `entrypoint` wrapper. If you want to do custom things to MySQL during its first run, or startup, then you'll want to modify the [MySQL entrypoint script](https://github.com/docker-library/mysql/blob/master/5.7/docker-entrypoint.sh). The one that I used is shown below with my comments inline. Basically, I wanted to add a parameter for my setup SQL script.

<script src="https://gist.github.com/emil10001/b98ce510330df7089067.js?file=sql_entrypoint.sh"></script>

You'll see on line 84 where I added the initialization from the script I passed in. You will also note that I added line 103, turning on `show_compatibility_56` in `/etc/mysql/my.cnf`. This was the solution to a problem that I had run into where whenever I tried to connect from my Java app as a non-root user, I was given an error like the following:

    SELECT command denied to user 'test'@'host' for table 'session_variables'

I ran across the [initial solution on StackOverflow](http://stackoverflow.com/a/32054183/974800), and was able to implement it in this entrypoint script.

## Tying it together

Below is my `buildrun.sh` script, which I use as a one-liner for setting everything up and running it. For the purposes of this post, it makes some sense, though for my actual implementation, I split the Java app from the SQL instance, so that I can iterate on my Java code without needing to constantly setup and teardown a MySQL instance. It's also set up such that both instances are ephemeral, since I get annoyed when Docker leaves around dozens of 300MB files on my relatively constrained SSD.

<script src="https://gist.github.com/emil10001/b98ce510330df7089067.js?file=buildrun.sh"></script>

The other issue that is common with running MySQL in Docker is one of data persistence. Unless you are keeping around the instances that you run, you're going to lose data in between runs if you don't do something to handle that. Data persistence is obviously an important aspect of a database. There are two options for keeping your data persistent:

1. Passing in a volume from host
1. Creating a share Docker volume

On my Mac, I had trouble with using a host volume for MySQL, I kept getting permissions errors, and it would fail to run after that. So, I opted for creating the shared Docker volume. To me, using a host volume would still be preferable, since I'd really like to be able to commit the data directory to my repository, so that I can share it with the code, but oh well. (Yes, I know that sounds strange and bad, but in my particular case, it does make some sense.)

The shared Docker volume is created on line 22, and explained in the [Docker documentation](https://docs.docker.com/userguide/dockervolumes/#creating-and-mounting-a-data-volume-container). In our run command, we call that volume on line 42.

## Conclusion

This was a bit more involved than I suspected, but I think that it was worth spending the time to get this up and running. The workflow is much nicer than needing to do this stuff natively, or constantly deploying to a remote host. When I came into the office on Monday morning, after getting this up and running over the weekend, the Docker portions without issue, and it saved me a ton of time. Hopefully this information saves you a bit of time as well.

[GitHub repo with source for this whole project.](https://github.com/emil10001/mysql-java-docker)
