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

