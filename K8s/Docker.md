# Docker

[TOC]

## Running containers

You can start a docker container with:

```bash
# '-ti' stands for 'terminal interactive'
docker run -ti ubuntu bash
```

When you exit, the container terminates. Port mapping can be added with:

```bash
# You can access this via http://localhost:8080. Nginx thinks it's running
# on port 80.
docker run -ti -p 8080:80 nginx
```

Injecting environment variables:

```bash
docker run -ti -p 8080:8080 -e VERSION=1 hello-world
```

Injecting environment variables allows you to decouple configuration from the application. This can also be used to pass in secrets so they don't get hardcoded into the image.

Attach to a running container with:

```bash
docker exec -ti <container_id> bash

# Then inspect the value of environment variables with
echo $ENV

# Note that this requires bash to be inside the container. In examples like busybox, bash isn't there, but sh is.
docker run -ti busybox sh
```

* `-p 8080:80` Assigns a port of 8080 on the outside to port 80 on the inside of the container
* `-rm` cleans up the container and removes the file system when it exits





## Creating an Image

Images are built from a `Dockerfile`.

Example 1:

```dockerfile
FROM node:8.12.0-slim
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 8080
CMD ["npm", "start"]
```

```bash
docker build -t hello-world .
docker run -ti -p 8080:8080 hello-world
```



Example 2:

```dockerfile
FROM ubuntu:20.10
RUN apt-get update
RUN apt-get -y install figlet
COPY ./copyme.txt /myfolder/copyme.txt
```

* `FROM` is the base image the container is built from.
* `RUN` is used to run commands during the built phase.
* `COPY` transfers local files into the container at build time. 
* `CMD` when the container starts, it executes this command.
* `EXPOSE 8080` doesn't expose anything. It's equivalent to a comment.





### Build an image

This builds an image, but does not run it.

```bash
# '-t first' is the name of the image. '.' is where to find the dockerfile
docker build -t first .
```

You can confirm the image has been built successfully with:

```bash
docker images | grep first
```

Then you can run it:

```bash
docker run -ti first

# And test with
figlet "it works"
```



## Removing Images

```bash
docker image rm <imagename:tag>
```







# Sharing the file system

Volumes are mapped using the `-v` flag. 

```bash
mkdir shared
echo "Hello World" > shared/test.txt
```

Mount the directory `./shared` as `myfolder` inside the container:

```bash
docker run -v ${PWD}/shared:/myfolder -ti busybox
```

Mount current directory into a folder:

```bash
docker run -ti -v ${PWD}:/data containerimage:tag
```





Containers may not contain shells, to avoid being exploitable. This is where you use 'kubectl debug -it '. Don't use SCRATCH, use distroless images. https://github.com/GoogleContainerTools/distroless. They are also quicker to scale.



http://crunchtools.com/comparison-linux-container-images/

A kernel panic in a container will take down the entire host. A VM would just shut itself down, because the kernel isn't shared in VMs.





Want to look at a docker image locally?  Save the image locally with `docker save app > app.tar` and inspect it.





Logging into the docker container registry so that you can push images there.

```bash
docker login
```

Tag an app appropriately:

```bash
docker tag app rafaelh/first-k8s-app:1
```

