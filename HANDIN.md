# Docker handin
By: Stephanie Rømer Hvenegaard

Handin date: 9 august.

class: T930007101 - Continuous Delivery and DevOps - Summer School
## Task 1. The multi-color TOIlet

the assingment: 

Write a build file that:

* takes an ubuntu 18.04 image as base
* instals toilet.
* The command `toilet -F border --gay` should always will be run before any user inputted commands to the container.
* If no user input is made, the default command should be "hello world"

the docker file can be found in folder named 1 

this DockerFile sould contain all requerments from the above list.

``` docker 
# Dockerfile for docker-handin 1 application

# Add a base image to build this image off of this case a ubuntu image versione 18.04
FROM ubuntu:18.04

# Runs commands on this base image creating a layer for each run comman.
RUN apt-get update -y && apt-get install -y \
    toilet

# add an entry point, this should always be run before any user inputted commands to the container.
ENTRYPOINT ["toilet", "-F", "border", "--gay"]

# Add a default command for this image
CMD ["Hello world"]
```

then to the following tasks : 

* Submit your Dockerfile file. 
* Build your image, calling it `<yourusername>/toilet`.
i am going to assume that these to refures to the same thing or the vary least that submitting is the overall game. 

i builded the above image with the folling command

```bash 
docker build -t <username>/toilet:latest .
```

i tag it with my user name to make sure the push will go as it should

* Push your newly created image up to dockerhub, and provide the link to it.

that is done with the following command
``` bash
docker push "<username>/toilet:latest" 
```

and it can be downloaded like this. 

``` bash
docker pull stephanienight/toilet
```

* Describe what you could do in order to: Minimize the layers in the image.

i whould nest the run commands in a single run instead of two, like this: 
``` bash 
RUN apt-get update -y && apt-get install -y \
    toilet
```

* Minimize the overall image size. You are allowed to change everything, as long as the following command `docker run <yourusername>/toilet hello world` successfully displays a colorful variation of `hello world` 

i whould use alpine as a base image. how ever i did not get this to work as i could not find the correct sh command for the package maneger. the reason i whould go with alpine is because that image in and of it self is one 5th the size of the ubuntu image.

## Task 2. Wordpress with proxy

We want to run a wordpress site, that sits behind a proxy server. You do not need any experience with proxies, nor Nginx in particular to solve this assignment.

**Tasks:**

You need to provide a docker compose file with the following containers in:

* Nginx as a proxy (in the script called loadbalancer-nginx)
* Mysql as a database (in the script called db)
* Wordpress as an application (in the script called wordpress)
* Make nginx the only container visible to the outside world, and only on port 80.
* Make the containers start in the following order: mysql,wordpress,nginx
* Make nginx volume in the file `nginx.conf` on the container path /etc/nginx/conf.d/default.conf
* Make wordpress and mysql configured so they do not need to ask for database host, database name, user and password (hint: look at their docker-hub pages)
* Make a network that all containers belong to.


Reflection tasks.
* Describe what kind of commands you would use to delete the containers and create new ones.
* Describe where you would define what exact version of mysql docker should use?
* What commands will give you the ip addresses of the containers in the described network.

## Task 3. Finding base images on Docker Hub


**Tasks**

* Make the same traversal of docker images for the `nextcloud` image with tag `19` found here:
https://hub.docker.com/_/nextcloud and Write in the hand-in the chain of images that nextcloud is dependent on.

well 
``` bash 
docker image inspect nextcloud:19
```
did not give much usefull after i used the 

``` bash
docker pull nextcloud:19 
```
to get the image, so instead i used the old facion way and found each docker file of each open source project the next cloud depended on. and this was the chain : 

``` bash
Nextcoud:19 
FROM php:7.4-apache-buster
FROM debian:buster-slim 
FROM scratch
```

that is the chain it took some manual work to get the results. 




