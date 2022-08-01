# Docker

## Commands

### [List all containers](https://docs.docker.com/engine/reference/commandline/ps/)

`docker ps` to list running containers  
`docker ps -a` to list all containers

### [Executes a command in a running container](https://docs.docker.com/engine/reference/commandline/exec/)
`docker exec`  
`sudo docker exec -it [container name] /bin/bash` run bash

### [Run docker image](https://docs.docker.com/engine/reference/run/)
`docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]`

### [Login to docker registry](https://docs.docker.com/engine/reference/commandline/login/)
`docker login [OPTIONS] [SERVER]`