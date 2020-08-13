# Docker handin
By: Stephanie Rømer Hvenegaard

Handin date: 9 august.

class: T930007101 - Continuous Delivery and DevOps - Summer School
## Task 1 The multi-color TOIlet

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
RUN apt-get update -y
RUN apt-get install toilet -y

# add an entry point, this should always be run before any user inputted commands to the container.
ENTRYPOINT toilet -F border --gay

# Add a default command for this image
CMD echo "Hello world"

```

then to the following tasks : 

* Submit your Dockerfile file. 
* Build your image, calling it `<yourusername>/toilet`.
i am going to assume that these to refures to the same thing or the vary least that submitting is the overall game. 
i builded the above image with the folling comman 
```bash 
docker build -t <username>/toilet:latest
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


* Describe what you could do in order to:
    * Minimize the layers in the image
    nest the run commands in a single run instead of two. 

    * Minimize the overall image size. You are allowed to change everything, as long as the following command `docker run <yourusername>/toilet hello world` successfully displays a colorful variation of `hello world` 
     i whould use alpine as a base image. how ever i did not get this to work as i could not find the correct sh command for the packege maneger.


