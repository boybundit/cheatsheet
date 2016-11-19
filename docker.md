# Docker

## Install

https://docs.docker.com/engine/installation/linux/ubuntulinux/

```bash
$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates
$ sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
$ echo "deb https://apt.dockerproject.org/repo ubuntu-xenial main" | sudo tee /etc/apt/sources.list.d/docker.list
$ sudo apt-get update
$ sudo apt-get install docker-engine

# Add yourself to docker group, to avoid having to use sudo when you use the docker command
# https://docs.docker.com/engine/installation/linux/ubuntulinux/#create-a-docker-group
$ sudo usermod -aG docker $(whoami)
# Then log out and log back in

$ docker version
$ docker ps
$ docker run hello-world
$ docker ps -a
```

## Usage

```bash
$ docker images
$ docker run -t -i ubuntu:14.04 /bin/bash
$ docker pull centos
$ docker run -t -i centos /bin/bash

$ docker search sinatra
```

## Dockerfile

```bash
$ touch Dockerfile
```
```Dockerfile
# This is a comment
FROM ubuntu:14.04
MAINTAINER Kate Smith <ksmith@example.com>
RUN apt-get update && apt-get install -y ruby ruby-dev
RUN gem install sinatra
```
```bash
$ docker build -t ouruser/sinatra:v2 .
$ docker run -t -i ouruser/sinatra:v2 /bin/bash

# Tag
$ docker tag 5db5f8471261 ouruser/sinatra:devel
$ docker images ouruser/sinatra

# Digest
$ docker images --digests | head
$ docker pull ouruser/sinatra@sha256:cbbf2f9a99b47fc460d422812b6a5adff7dfee951d8fa2e4a98caa0382cfbdbf

# Docker Hub
$ docker push ouruser/sinatra

# Remove image
$ docker rmi training/sinatra
```
