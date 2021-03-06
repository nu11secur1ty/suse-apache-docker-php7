[ ![Codeship Status for novacoast/opensuse-apache-docker](https://codeship.com/projects/b0bd21b0-5274-0132-69f7-72279d09a1d7/status)](https://codeship.com/projects/48655) [![Code Climate](https://codeclimate.com/github/novacoast/opensuse-apache-docker/badges/gpa.svg)](https://codeclimate.com/github/novacoast/opensuse-apache-docker)


# Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Upgrading](#upgrading)
- [Docker Compose](#docker-compose)
- [Contributing](#contributing)
- [need_to_install](#need_to_install)
- [start_service](#start_service)




# need_to_install:
```bash
zypper -n in docker
zypper -n in git
```
# start_service:
```bash
rcdocker start
```
# Introduction

Dockerfile to build an OpenSUSE 42.3 container image with apache2 and php7.

## Features:
- Apache2
- php7 & common modules
- php.ini configured to utilize `getenv()`

# Quick Start

- Pull image from docker
```bash
docker pull nu11secur1ty/suse-apache-docker-php7
```
- Run the opensuse-apache-docker image

```bash
docker run -d -p 8080:80 nu11secur1ty/suse-apache-docker-php7
```
- Output
```bash 
http://localhost:8080/
```
![](https://github.com/nu11secur1ty/suse-apache-docker-php7/blob/master/screen/screen.png)

- - - Terminal access to the docker
```bash
docker run -t -i nu11secur1ty/suse-apache-docker-php7 /bin/bash
```

# Check for docker running containers:
```bash
docker container ls
docker ps -a
```
# Check for docker images:
```bash
docker images
```

# Installation

Pull the latest version of the image from the docker index. These builds are performed by the **Docker Trusted Build** service.

https://hub.docker.com/r/nu11secur1ty/suse-apache-docker-php7

```bash
docker pull nu11secur1ty/opensuse-apache-docker-php7:latest
```

Alternately you can build the image yourself.

```bash
git clone https://github.com/nu11secur1ty/suse-apache-docker-php7.git
cd suse-apache-docker-php7
docker build -t="$USER/suse-apache-docker-php7" .
```

# Upgrading

To upgrade to newer releases, simply follow this 3 step upgrade procedure.

- **Step 1**: Stop the currently running image

```bash
docker stop suse-apache-docker-php7
```

- **Step 2**: Update the docker image.

```bash
docker pull nu11secur1ty/suse-apache-docker-php7:latest
```

- **Step 3**: Start the image

```bash
docker run --name='suse-apache' -d -p 8080:80 nu11secur1ty/suse-apache-docker-php7
echo "or"
docker run --name='suse-apache' -d -p 80:80 nu11secur1ty/suse-apache-docker-php7
```
---------------------------------------------------------------------------------

# Docker-Compose

Build and run using [docker-compose](https://github.com/docker/compose)

```bash
zypper in docker-compose
git clone https://github.com/nu11secur1ty/suse-apache-docker-php7.git
cd suse-apache-docker-php7
docker-compose build
```
- Alias
```bash
docker-compose up
```

- when you make a changes 
```bash
docker-compose build
```
- run your container
```bash
docker run -d -p 8080:80 _your_image
```
- Image example:
Removing intermediate container 862428281d57
 `---> b39e0d40b7e4`
Successfully built `b39e0d40b7e4`
Successfully tagged suseapachedockerphp7_opensuseapache-php7:latest

# Stop docker
- Scan for dockers which you want to stop
```bash
docker container ls
```
- stop your docker
```bash
docker stop 4a80d39c80e1
```
- Remove image
```bash
docker rmi 4a80d39c80e1
```
-------------------------------------------------------------------------------------------------------------------
# How do I SSH into a running container
***There is a docker exec command that can be used to connect to a container that is already running.*** 
 
   - Use docker ps to get the name of the existing container
   - Use the command `docker exec -it <container name> /bin/bash` to get a bash shell in the container
   - Generically, use `docker exec -it <container name> <command>` to execute whatever command you specify in the container.

# How do I run a command in my container?

The proper way to run a command in a container is: `docker-compose run <container name> <command>`. For example, to get a shell into your web container you might run docker-compose run web `/bin/bash`

To run a series of commands, you must wrap them in a single command using a shell. For example: `docker-compose run 
<name in yml> sh -c '<command 1> && <command 2> && <command 3>'`

In some cases you may want to run a container that is not defined by a docker-compose.yml file, for example to test a new container configuration. Use docker run to start a new container with a given image: `docker run -it <image name> <command>`

The docker run command accepts command line options to specify volume mounts, environment variables, the working directory, and more.

# Getting a shell for build/tooling operations

Getting a shell into a build container to execute any operations is the simplest approach. You simply want to get access to the `cli` container we defined in the compose file. The command `docker-compose -f build.yml run cli` will start an instance of the `phase2/devtools-build` image and run a bash shell for you. From there you are free to use `drush`, `grunt` or whatever your little heart desires.


# Running commands, but not from a dedicated shell

Another concept in the Docker world is starting a container to run a single command and allowing the container stop when the command is completed. This is great if you run commands infrequently, or don't want to have another container constantly running. Running your commands on containers in this fashion is also well suited for commands that don't generate any files on the filesystem or if they do, they write those files on to volumes mounted into the container.

The `drush` container defined in the example `build.yml` file is a container designed specifically to run drush in a single working directory taking only the commands as arguments. This approach allows us to provide a quick and easy mechanism for running any drush command, such as `sqlc`, `cache-rebuild`, and others, in your Drupal site quick and easily.

There are also other examples of a `grunt` command container similar to `drush` and an even more specific command container around running a single command, `drush make` to build the site from a make/dependency file.

-----------------------------------------------------------------------------------------------------------------------------

The [webapp](webapp) folder on the host will be mounted into the container's apache root

# Contributing

+ Report Issues
+ Open a Pull Request


------------------------------------------------------------------------------------------------------


# Docker provides a single command that will clean up any resources — images, containers, volumes, and networks — that are dangling (not associated with a container):

```bash
docker system prune
```


# To additionally remove any stopped containers and all unused images (not just dangling images), add the -a flag to the command:

```bash 
docker system prune -a
```

# Remove one or more specific images

Use the docker images command with the -a flag to locate the ID of the images you want to remove. This will show you every image, including intermediate image layers. When you've located the images you want to delete, you can pass their ID or tag to docker rmi:

- **List:**
```bash
docker images -a
```
- **Remove:**
```bash 
docker rmi Image Image
```
# Remove dangling images

Docker images consist of multiple layers. Dangling images are layers that have no relationship to any tagged images. They no longer serve a purpose and consume disk space. They can be located by adding the filter flag, -f with a value of dangling=true to the docker images command. When you're sure you want to delete them, you can use the docker images purge command:

**NOTE**
```txt
If you build an image without tagging it, the image will appear on the list of dangling images because it has no association with a tagged image. You can avoid this situation by providing a tag when you build, and you can retroactively tag an images with the docker tag command.
```
- **List:**
```bash
docker images -f dangling=true
```
- **Remove:**
```
docker images purge
```
# Removing images according to a pattern

You can find all the images that match a pattern using a combination of docker images and grep. Once you're satisfied, you can delete them by using awk to pass the IDs to docker rmi. Note that these utilities are not supplied by Docker and are not necessarily available on all systems:

- **List:**
```bash
docker images -a |  grep "pattern"
```

- **Remove:**
```bash
docker images -a | grep "pattern" | awk '{print $3}' | xargs docker rmi
```
# Remove all images

All the Docker images on a system can be listed by adding -a to the docker images command. Once you're sure you want to delete them all, you can add the -q flag to pass the Image ID to docker rmi:

- **List:**
```bash
docker images -a
```
- **Remove:**
```bash
docker rmi $(docker images -a -q)
```
# Remove a container upon exit

If you know when you’re creating a container that you won’t want to keep it around once you’re done, you can run docker run --rm to automatically delete it when it exits.

- **Run and Remove:**
```bash
 docker run --rm image_name
```
# Remove all exited containers

You can locate containers using docker ps -a and filter them by their status: created, restarting, running, paused, or exited. To review the list of exited containers, use the -f flag to filter based on status. When you've verified you want to remove those containers, using -q to pass the IDs to the docker rm command.

- **List:**
```bash
docker ps -a -f status=exited
```
- **Remove:**
```bash
docker rm $(docker ps -a -f status=exited -q)
```
# Remove containers using more than one filter

Docker filters can be combined by repeating the filter flag with an additional value. This results in a list of containers that meet either condition. For example, if you want to delete all containers marked as either Created (a state which can result when you run a container with an invalid command) or Exited, you can use two filters:

- **List:**
```bash
docker ps -a -f status=exited -f status=created
```
- **Remove:**
```bash
docker rm $(docker ps -a -f status=exited -f status=created -q)
```
# Remove containers according to a pattern

You can find all the containers that match a pattern using a combination of docker ps and grep. When you're satisfied that you have the list you want to delete, you can use awk and xargs to supply the ID to docker rmi. Note that these utilities are not supplied by Docker and not necessarily available on all systems:

- **List:**
```bash
docker ps -a |  grep "pattern”
```
- **Remove:**
```bash
docker ps -a | grep "pattern" | awk '{print $3}' | xargs docker rmi
```
# Stop and remove all containers

You can review the containers on your system with docker ps. Adding the -a flag will show all containers. When you're sure you want to delete them, you can add the -q flag to supply the IDs to the docker stop and docker rm commands:

- **List:**
```bash
docker ps -a
```
- **Remove:**
```bash
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```
# Removing Volumes
Remove one or more specific volumes - Docker 1.9 and later

Use the docker volume ls command to locate the volume name or names you wish to delete. Then you can remove one or more volumes with the docker volume rm command:

- **List:**
```bash
docker volume ls
```
- **Remove:**
```bash
docker volume rm volume_name volume_name
```
# Remove dangling volumes - Docker 1.9 and later

Since the point of volumes is to exist independent from containers, when a container is removed, a volume is not automatically removed at the same time. When a volume exists and is no longer connected to any containers, it's called a dangling volume. To locate them to confirm you want to remove them, you can use the docker volume ls command with a filter to limit the results to dangling volumes. When you're satisfied with the list, you can remove them all with docker volume prune:

- **List:**
```bash
docker volume ls -f dangling=true
```
- **Remove:**
```
docker volume prune
```
# Remove a container and its volume

If you created an unnamed volume, it can be deleted at the same time as the container with the -v flag. Note that this only works with unnamed volumes. When the container is successfully removed, its ID is displayed. Note that no reference is made to the removal of the volume. If it is unnamed, it is silently removed from the system. If it is named, it silently stays present.

- **Remove:**
```bash
docker rm -v container_name
```
# Conclusion

This guide covers some of the common commands used to remove images, containers, and volumes with Docker. There are many other combinations and flags that can be used with each. For a comprehensive guide to what's available, see the Docker documentation for docker system prune, docker rmi, docker rm and docker volume rm. If there are common cleanup tasks you'd like to see in the guide, please ask or make suggestions in the comments.

------------------------------------------------------------------------------------------------------------------------

# Vulnerabilities

![](https://github.com/nu11secur1ty/opensuse-apache-docker/blob/master/Screenshot%20from%202018-05-12%2020-31-11.png)

# Packages providing

![](https://github.com/nu11secur1ty/opensuse-apache-docker/blob/master/Screenshot%20from%202018-05-12%2020-32-12.png)

# Good luck to everyone! ;)
