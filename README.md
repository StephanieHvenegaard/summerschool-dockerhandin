# Docker exercise

If you are unable to do any of the tasks asked of you here, then please provide a description of what you have _tried_ to do, or what you would do _if it worked_.

## 1. The multi-color TOIlet

The TOIlet (stands for “The Other Implementation’s letters”) application is a linux CLI that takes the input characters and produces various text-based effects.

Construct a container that can output me colorful variations of a given text.

See [TOIlet's website](http://caca.zoy.org/wiki/toilet) for more info.

**Tasks:**

Write a build file that:

* takes an ubuntu 18.04 image as base
* instals toilet.
* The command `toilet -F border --gay` should always will be run before any user inputted commands to the container.
* If no user input is made, the default command should be "hello world"

Submit your Dockerfile file.

Build your image, calling it `<yourusername>/toilet`.

Push your newly created image up to dockerhub, and provide the link to it.

Describe what you could do in order to:

* Minimize the layers in the image
* Minimize the overall image size. You are allowed to change everything, as long as the following command `docker run <yourusername>/toilet hello world` successfully displays a colorful variation of `hello world`

## 2. Wordpress with proxy

We want to run a wordpress site, that sits behind a proxy server. You do not need any experience with proxies, nor Nginx in particular to solve this assignment.

You need to provide a docker compose file with the following containers in:

* Nginx as a proxy (in the script called loadbalancer-nginx)
* Mysql as a database (in the script called db)
* Wordpress as an application (in the script called wordpress)

**Tasks:**

* Make nginx the only container visible to the outside world, and only on port 80.
* Make the containers start in the following order: mysql,wordpress,nginx
* Make nginx volume in the file `nginx.conf` on the container path /etc/nginx/conf.d/default.conf
* Make wordpress and mysql configured so they do not need to ask for database host, database name, user and password (hint: look at their docker-hub pages)
* Make a network that all containers belong to.

Describe what kind of commands you would use to delete the containers and create new ones.

Describe where you would define what exact version of mysql docker should use?

What commands will give you the ip addresses of the containers in the described network.

## 3. Finding base images on Docker Hub

As we know, Docker images are either based on other images or the `scratch` image that is empty.

As an example, the `postgres` image in latest version https://hub.docker.com/_/postgres is based on `debian` image with the `buster-slim` tag, which again is based on `scratch`.

In that way, postgres is leveraging the work done by Debian.

**Tasks**

Make the same traversal of docker images for the `nextcloud` image with tag `19` found here:
https://hub.docker.com/_/nextcloud

Write in the hand-in the chain of images that nextcloud is dependent on.
