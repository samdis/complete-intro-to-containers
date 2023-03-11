# Containers

## Get the linux dist you are running on
```zsh
cat etc/issue
```

## Get the bash deps
```zsh
ldd bin/bash # ldd = list dynamic dependencies
```

## Get out of the chroot env
```zsh
exit
```

ps = processes
ps aux = give me more results

cgroups = control group
- limit the resources for a sub-process

htop 
- cool tool for seeing what your computer is up to

`docker exec -it <container-name>`
- connect to container with interactive shell (it = interactive )
- `it` = `--interactive --tty` tty = Teletype


`yes`
- just repeats yes or whatevers next forever and ever

# Docker 

`docker export -o file-name.tar container-name`
- export the container as tarball


`docker run -it image-name`
- run the docker image

`docker run -it -detach ubuntu:bionic`
- run the docker image in a detached state

`docker attach my-image-name`
- attach to a backgrounded docker image

`--rm` 
- cleans up the docker container after you exit
- instead of having to run `docker rm my-container-name`

## Tags
- if you dont specify a tag, it assumed `:latest`

## Docker CLI

`docker pull image-name`
- allows you to pre-fetch a container to run


`jturpin/hollywood` looks like a holywood hacker haha

`docker inspect`
- gives you information about the image

`docker pause` `docker unpause`
- pause a specific container

`docker kill`
- kill the running image

`docker exec`
- find a container to execute a command on

`docker history`
- shows modifications of the container

`docker info` 
- dumps information about whats running the container

`docker ps --all`
- show all images (including stopped)

`docker container prune`
- remove all stopped containers

# Dockerfile
`docker build --tag my-node-app .`
- add tag name of my-node-app
- add version by my-node-app:1
- `-t` for short

`docker images` 
- see all images

`--init`
- allows you to kill the process with `cmd + c`

`docker run --init --publish 3000:3000 my-node-app`
- expose on port 3000

`ADD`
- can go and download from a network versus `COPY` which is just files

# Making tiny containers

`docker inspect alpine:3.10 | jq` 
- colorize output
- see details about the image

`RUN addgroup -S node && adduser -S node -G node`
- add a user node in the group node


## Multi stage builds
`FROM imageName AS stage1`
- alias stage to name
  
`COPY --from=stage1 --chown=node:node /build .`
- refer to stage 1 build and copy over while changing ownership

`docker build -t -f fileName .`
- refer to a specific dockerfile in a dir
- `-f` = file

# Docker features

Note: you want cattle, not snowflakes (remember analogy)

## Bind mounts
- portals to you host computer

`docker run --mount type=bind,source="$(pwd)"/build,target=/usr/share/nginx/html -p 8080:80 nginx`
 - bind from my local (fully qualified path), to this directory within the container

## Volumes
- hold onto these files, and the name of these files are "name"
- always prefer volumes over bind mounts

`docker run --env DATA_PATH=/data/num.txt --mount type=volume,src=incrementor-data,target=/data incrementor`

## Dev environment
`docker run --rm -it --mount type=bind,source="$(pwd)",target=/src -p 1313:1313 -u hugo jguyomard/hugo-builder:0.55 hugo server -w --bind=0.0.0.0`

## Networks
`docker network create --driver=bridge app-net`
- create a bridge network so apps can speak to eachother

`docker run -d --network=app-net -p 27017:27017 --name=db --rm mongo:3`
connect to through
`docker run -it --network=app-net --rm mongo:3 mongo --host db`
- this is how you do it manually, but you seldom ever do this manually

# Docker Compose
- not really suitable for production
  - only works for a single host
- useful in ci/cd

# Kubernetes
- master
  - central control plane - the brain
- nodes
  - one node can have multiple containers
- pod
  - a deployment unit that can not be seperated
- service
  - group of pods
- 