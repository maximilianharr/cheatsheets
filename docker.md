# Docker Cheatsheat
A [Container](https://www.docker.com/resources/what-container) is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings. [1]

If you run on Ubuntu you can use [this script](https://github.com/maximilianharr/code_snippets/blob/master/sh/install_docker.sh) to install docker and docker-compose.

## Installation

### Check Docker Installation/Version
```bash
docker --version
docker run hello-world
```

### Check Docker-Compose Version
```bash
docker-compose --version
```

## Containers

### Show all containers
```bash
docker ps -a
```

### Remove all containers
```bash
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```

### Get IP address of docker container
```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ${CONTAINER_NAME}
```