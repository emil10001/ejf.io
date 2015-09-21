+++
date = "2015-09-20T20:50:55-07:00"
title = "Docker Cleanup Commands"

+++

Running docker, creating and running containers can make a bit of a mess, depending on how you use it. After a couple weeks of using Docker for development, I figured that it would be good to figure out how to clean up my unused images and containers.

## Images

To remove an image called 'node'

    docker rmi node

To remove all untagged images

    docker rmi $(docker images -a | grep "^<none>" | awk '{print $3}')

To remove all images

    docker rmi $(docker images -qf "dangling=true")

## Containers

To list active containers

    docker ps -a

To remove all stopped containers

    docker rm $(docker ps -a -q)

To remove all existing containers

    docker rm $(docker ps -aq)

To kill and remove containers

    docker rm $(docker kill $(docker ps -aq))

## References

* http://jimhoskins.com/2013/07/27/remove-untagged-docker-images.html
* http://stackoverflow.com/a/17870293/974800
* http://stackoverflow.com/a/19711913/974800
* https://developer.basespace.illumina.com/docs/content/documentation/native-apps/manage-docker-image
