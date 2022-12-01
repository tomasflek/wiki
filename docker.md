# Docker

## Commands

[List all containers](https://docs.docker.com/engine/reference/commandline/ps/)

`docker ps` to list running containers  
`docker ps -a` to list all containers

[Executes a command in a running container](https://docs.docker.com/engine/reference/commandline/exec/)  
`docker exec`  
`sudo docker exec -it [container name] /bin/bash` run bash

[Run docker image](https://docs.docker.com/engine/reference/run/)  
`docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]`

[Login to docker registry](https://docs.docker.com/engine/reference/commandline/login/)  
`docker login [OPTIONS] [SERVER]`

[Fetch docker logs](https://docs.docker.com/engine/reference/commandline/logs/)  
`docker logs [OPTIONS] CONTAINER`

[Check docker configuration](https://docs.docker.com/engine/reference/commandline/inspect/)  
` docker inspect [OPTIONS] NAME|ID [NAME|ID...]`

Stop running image  
`docker stop` 

Docker provides a single command that will clean up any resources — images, containers, volumes, and networks — that are dangling (not tagged or associated with a container)  
`docker system prune`

To additionally remove any stopped containers and all unused images (not just dangling images), add the -a flag to the command.  
`docker system prune -a`

Save docker image to tar file  
`docker image save -o images.tar home-organizer`