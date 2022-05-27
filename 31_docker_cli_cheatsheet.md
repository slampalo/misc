# **Palowan Guru Cheatsheet - Docker CLI **

## **Table of Contents**

0. [TL;DR](#tldr) 
> If you are looking only for the syntax or the functionality, go directly to the TL;DR section. (Thanks Mr. Kierr Sunega for the suggestion)
1. [Image](#image)
2. [Volume](#volume)
3. [From image to container](#from-image-to-container)
4. [Container](#container)
5. [Docker Compose](#docker-compose)
6. [References](#references) 

---

> **NOTES**
> 
> 1. For the sake of consistency, here is the structure of commands in this cheat sheet: 
>     - `command [OPTIONS] PARAMETER`
>       - Commands are indicated in lowercase.
>       - Parameters are indicated in uppercase.
>       - Parameters inside square brackets "[ ]" are optional.
>
> 2. You may know more about the command by typing `command --help` or `man command`
>     - for example:
>       - `docker run --help` or `man docker run` displays the user manual of command `docker run`


---

## **TL;DR**

**Image**

- List all images: `docker images`
- Build image: `docker build -t IMAGE_NAME:TAG PATH`
- Remove image: `docker rmi -f IMAGE_NAME`
- Remove all unused images: `docker image prune`
- Remove all images: `docker image prune -a`

**Volume**

- List all volumes: `docker volume list`
- Display information: `docker volume inspect VOLUME_NAME`
- Create a volume: `docker volume create VOLUME_NAME`
- Remove volume: `docker volume rm VOLUME_NAME`
- Remove all unmounted volumes: `docker volume prune`


**Container**

- List all running containers: `docker ps`
- List all containers: `docker ps -a`
- Log container console: `docker logs CONTAINER_NAME`
- Create and run a container: `docker run --name CONTAINER_NAME IMAGE`
- Remove container `docker rm CONTAINER_NAME`
- Remove all stopped containers: `docker container prune`

**System**

- Remove all unused data: `docker system prune`

**Docker compose**

- Create services: `docker-compose create`
- Start services: `docker-compose start`
- Create containers and immediately start services: `docker-compose up`
- pause services: `docker-compose pause`
- unpause services: `docker-compose unpause`
- Stop services: `docker-compose stop`
- Remove stopped service containers: `docker-compose rm`
- Stop and remove containers: `docker-compose down`

---

## **Docker**

Virtualisation
- Container
  - OS virtualisation
  - process isolation
- Virtual Machine
  - Hardware virtualisation  
  (Virtual environments are built on top of the hardware level, so it necessarily includes the OS, hardware config, etc in virtual environment)
  - machine isolation

Implementations of containerisation
<!-- keywords: chroot, cgroup, isolated namespaces -->
- LXC
- Docker

---

## **Image**

`docker pull IMAGE` = Download the image (default from [DockerHub](https://hub.docker.com/))

`docker images` = List all images

`docker build [OPTIONS] PATH` = Create an image from a dockerfile
- `--no-cache` = Build image without using cache
- `-t IMAGE_NAME:TAG` = Custom image name and tag

`docker rmi -f IMAGE_NAME` = Remove an image

`docker image prune [OPTIONS]` = Remove all unused images
- `-a` = Remove ALL images
- `-f` = Remove without confirmation

`docker save IMAGE | pv > IMAGE_FILE` = Export image to image file

`docker load IMAGE_FILE` = Import image from image file

---

## **Volume**

> Volume is an external filesystem connecting to container(s) that allows us storing data inside container persistently. Since it is an external system, it is not affected by the lifecycle of the container. In other words, the data is accessible after the container was stopped, killed or removed. A volume can be mounted to a container by adding `--mount` or `-v` to `docker run` or `docker create` commands.

`docker volume create VOLUME_NAME` = Create a volume

`docker volume list` = List all volumes

`docker volume inspect VOLUME_NAME` = Display information about the volume

`docker volume rm [OPTIONS] VOLUME_NAME` = Remove a volume
- `-f` = Remove without confirmation

`docker volume prune [OPTIONS]` = Remove all unmounted local volumes
- `-f` = Remove without confirmation

<!-- TODO　permission -->

---

## **From image to container**

`docker create [OPTIONS] IMAGE` = ONLY create a container from a docker image
- `--name CONTAINER_NAME` = Custom container name

`docker start CONTAINER_ID_or_NAME` = Run the containerised application (or re-start from the beginning)

`docker run [OPTIONS] IMAGE ` = Create a container from a docker image and start it immediately
- `docker run -it [OPTIONS] IMAGE TERMINAL` = Run image with interactive mode
- `--mount source=VOLUME_NAME, destination=PATH` = mount a volume to the directory inside container
- `--name CONTAINER_NAME` = Custom container name
- `--rm` = Automatically remove container when it exits

---

## **Container**

`docker ps [OPTIONS]` = List containers in progress

- `-a` = List all containers
- `--filter=FILTER` = List all filtered containers
  - some useful filter: status=created, status=running, status=exited, status=paused
- `-l` = Show the latest created container

`docker exec CONTAINER_ID_or_NAME COMMAND` = Run a command in a running container

`docker pause CONTAINER_ID_or_NAME` = Pause the running container (not to stop it)

`docker unpause CONTAINER_ID_or_NAME` = Resume the paused container (not to re-start it)

`docker stop [OPTIONS] CONTAINER_ID_or_NAME` = Terminate the running container (or kill it if no response after 10 seconds)
- `-t SECONDS` = Seconds for waiting before killing it

`docker rename CONTAINER_ID_or_NAME NEW_CONTAINER_NAME` = Rename a container

`docker rm CONTAINER_ID_or_NAME` = Remove container
- `-f` = Remove without confirmation

`docker container prune [OPTIONS]` = Remove all stopped containers
- `-f` = Remove without confirmation

`docker logs CONTAINER_ID_or_NAME` = Log container console

## **Docker system**

`docker [OPTIONS] df` = Display information on space usage
- `-v` = Display detailed information


`docker event` = Display real-time events for the docker daemon 

`docker system prune` = Remove all unused data
- `-a` = Remove ALL data
- `-f` = Remove without confirmation
- `--volumes` = Remove all unmounted volumes

---

<!-- ## **Context** -->

<!-- TODO -->

## **Docker Compose**

> Normally, each container serves ONLY ONE purpose. That is to say, for example, a database service should not be contained in the container running the server. Since every container is a isolated run-time environment, how could we integrate multiple services into one application if multipurpose container is always a bad idea? Well, that's why docker-compose comes into play.  
> Docker-compose is the tool that allows us to write only ONE docker-compose.yml file where we define the services needed and to run all containers at the same time with just one single command.  
> (Please note that you still have to write dockerfile for your custom image.)
<!-- keywords: networking -->

```yaml
# docker-compose.yml example

version: "3"
services:
  api_service:
    build: ./api_service
    env_file:
      - .env
    environment:
      NODE_ENV: production
    ports:
      - "8100:80"
    volumes:
      - .:/code
    depends_on:
      - database

database:
  image: 'postgres:14'
  env_file:
    - docker.env
  environment:
    POSTGRES_PORT: 5432
  volumes:
    - ./pgdata:/var/lib/postgresql/data

# explanation:

version: # Top-level element, specify the version of docker-compose tool, optional

services: # Top-level element, all services go under this field

  api_service: # service name, custom but should be semantically meaningful (api_service in the above example)

    image: # prebuilt image

    build: # directory that storing docker file from which the custom image for the service is built

    env_file: # file that storing environment variables

    environment: #  you may place some environment variables under this field (no credentials or sensitives)

    ports: # map the host port to the port of container (8100 and 80 respectively in this case)

    volumes: # As said above, volumes is a filesystem external to the container storing persistent data. By mounting the current directory . to the working directory inside the container (/code in this case), we can modify the code without re-building the image.

    depends_on: # services that the current service depends on
```

> Note: Go to the directory where the file compose.yaml is stored and run the following command:

`docker-compose create` = Create services

`docker-compose start` = Start services

`docker-compose up` = Create containers and immediately start services

`docker-compose pause` = pause services

`docker-compose unpause` = unpause services

`docker-compose stop` = Stop services

`docker-compose rm` = Remove stopped service containers

`docker-compose down` = Stop and remove containers

---

## References:

1. Docker  
   [Containers vs Virtual machines](https://www.ibm.com/cloud/blog/containers-vs-vms) \
   [Docker image and container](https://phoenixnap.com/kb/docker-image-vs-container) \
   [Container lifecycle](https://linuxhandbook.com/container-lifecycle-docker-commands/) \
   [Docker volumes](https://phoenixnap.com/kb/docker-volumes)

#### Brought to you by Suen Lam ;)

