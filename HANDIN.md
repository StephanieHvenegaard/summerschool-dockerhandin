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

all of the above reqirements has been handled with the docker-compose file posted here 
``` docker 
version: '3'
# defines the services.
services:
  # define the wordpress container
  wordpress:
    image: wordpress
    restart: always
    environment:
      - WORDPRESS_DB_USER=wordpress
      - WORDPRESS_DB_PASSWORD=wordpress
      - WORDPRESS_DB_PASSWORD=supersecretpassword
      - WORDPRESS_DB_HOST=db
    depends_on:
      - db
    networks:
      - wordpressnetwork

  # define the myswl container
  db:
    image: mysql:5.7
    restart: always
    environment:
      - MYSQL_USER=wordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_ROOT_PASSWORD=wordpress
      - MYSQL_PASSWORD=supersecretpassword
    networks:
      - wordpressnetwork

  # define the nginx-container
  loadbalancer-nginx:
    image: nginx
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    ports:
        - 80:80  
    depends_on: 
      - wordpress  
    networks:
      - wordpressnetwork
    
# defines the volumes
#volumes:
#  nginxvolume: {}

# define the networks  
networks:
  wordpressnetwork:
```

Reflection tasks.
* Describe what kind of commands you would use to delete the containers and create new ones.
``` docker 
docker-compose down
``` 
stops the containers and removes them maybe trow in a 'docker system prune' for good measure to be sure all that is not currently running gets wiped. 

``` docker 
docker-compose up
``` 
creates new ones

* Describe where you would define what exact version of mysql docker should use?
``` docker 
  # define the myswl container
  db:
    image: mysql:5.7 # right here, mysql version 5.7
    restart: always
    environment:
``` 
specify the images tag for the exact version i want to use.

* What commands will give you the ip addresses of the containers in the described network.

with the following command i totally did not find on stack overflow.
```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id 
```
to find the containers id user `docker ps`

``` 
[ubuntu@academysdu-instance29 2 ]$ docker ps
CONTAINER ID        IMAGE                                ...                                              
1f0ace3833ec        nginx                                ...
5fa171d1e17e        wordpress                            ...
920d4e41b6c9        mysql:5.7                            ...

```
then use the id's you dont need to type them all just enought to be unique, 2 characters was enough.

```
[ubuntu@academysdu-instance29 2 ]$ docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' 1f
172.20.0.4
[ubuntu@academysdu-instance29 2 ]$ docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' 5f
172.20.0.3
[ubuntu@academysdu-instance29 2 ]$ docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' 92
172.20.0.2

```


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




