# Docker

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
