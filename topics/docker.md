# Overview

Docker is used to run apps/processes without actually installing them. This is useful because it ensures the same environment is running on all the machines using the images that make the containers.

```
Dockerfile > (build) Image > (run) Container.
```

**Image** - A template for an environment, a snapshot of a system at a certain time. It has the OS, software and application code all bundled in one file.

**Container** - A running instance of an image.

**Dockerfile** - List of steps to build an image. Ex. configure the OS, install packages, copy project files into the right places...

# Install

Add the GPG key.

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Add the Docker repository to APT sources.

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

Update the package list to include Docker.

```
sudo apt-get update
```

Install Docker.

```
sudo apt-get install -y docker-ce
```

# Commands

There should be **only one** process per container.

Localhost (127.0.0.1) for a container is itself. Containers can access each other by using their names as the domain.

Docker uses a `unix socket` which means that it runs as `root` and asks for `sudo`.

## List

```bash
# List all running containers
docker ps

# List all containers, including stopped ones.
docker ps -a
```

## Run

```bash
# Build a container from an image and run it.
docker run <FLAGS> <IMAGE>

# Start an existing container by name or id.
docker start <NAME/ID>

# Stop a container by name or id.
docker stop <NAME/ID>

# Delete a container by name or id.
docker rm <NAME/ID>

# Remove all containers.
sudo docker rm $(sudo docker ps -aq)
```

## Inside

```bash
# Enter a container to execute commands.
docker exec -it <CONTAINER> bash
```

# Flags

```bash
# Container name
--name

# Interactive i.e. leave standard input open
-i

# Terminal
-t

# Map the container's `80` port, to the webserver's `8080` port i.e. the container can be accessed via `8080`.
-p 8080:80

# Run as daemon i.e. detached/background process.
-d

# Volume. Mount a local directory as a container directory. Used when not specified in Dockerfile.
-v <SOURCE>:<DESTINATION>
--mount source=myvol,target=/vol
```

# Dockerfile

Used to build images

```bash
# Always at top. Uses an image as a base.
FROM <IMAGE> -

# Run commands on image **build**. Can be chained with &&.
RUN <COMMAND> -

# Run a command on container **start**.
CMD ["<COMMAND", "<ARGS>">] -

# Copy from local directory to container directory.
COPY <SOURCE> <DESTINATION> -

# Set work directory. Default is /.
WORKDIR <PATH> -

# Expose a specific port.
EXPOSE <PORT> -
```

Build an image by running all commands in the Dockerfile.

```
docker build -t <NAME> <DOCKERFILE>
```

# Dockerfile example

```Dockerfile
FROM nginx
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
COPY /src /www
```

```
# ./nginx.conf - Bare minimum needed to run nginx.
server {
    root /www;
}
```

Build the image.

```
sudo docker build -t nginx-static
```

Build the container.

```bash
sudo docker run -d -p 8080:80 --name nginx-static nginx-static

# For this to work, a port forwarding rule is needed on the VM with host `3000` and guest `8080`. Container `80` > VM `8080` > Host `3000`.
```

Visit the website in the host via `127.0.0.1:3000`.

# Volume

-   Independent from containers
-   Mapped to storage
-   Multiple containers can access the same volume

## Commands

```bash
# list all volumes
docker volume ls

# create volume
docker volume create volume_name

# inpsect
docker volume inspect volume_name

# delete
docker volume rm volume_name
```

## Example

1. Create volume
2. Create container
3. Mount volume to a container

```bash
# Create volume
docker volume create myvol

# Start container and mount volume
docker run -itd --name test -v myvol:/vol alpine

# Go inside container
docker exec -it test bash

# Add data to volume
touch /vol/example.txt

# Check volume
cat /vol/example.txt
```

# Docker Compose

After a while, running, starting and stopping containers gets very tedious. Instead of manually typing the docker commands one by one, we can define a `docker-compose.yml` configuration file to do it for us.

We can either use pre-made Dockerfiles, or we can write their content directly in the compose file.

**All the locations are relative to the docker-compose file.**

```yml
version: "3" # Version of compose file format.

services:
    products-service: # This can be named anyway. Used for connecting containers.
        build: ./products # Folder containing Dockerfile.
        volumes: # List of volumes to be mounted.
            - ./products:/usr/src/app # Source:Container
        ports: # List of ports.
            - 5001:80 # Host:Container port forwarding.
```

## Install

```
sudo apt-get install docker-compose
```

## Commands

```bash
# Recreate image.
sudo docker-compose build

#  Recreate without cache.
--no-cache

# Run container.
sudo docker-compose up

# Stop and remove container.
sudo docker-compose dowm
```

## Container Networking

```yml
version: "3"
services:
    web:
        build: .
        ports:
            - "8000:8000"
    db:
        image: postgres
        ports:
            - "8001:5432"
```

When you run docker-compose up, the following happens:

-   A network called `myapp_default` is created.
-   A container is created using web's configuration. It joins the network `myapp_default` under the name `web`.
-   A container is created using db's configuration. It joins the network `myapp_default` under the name `db`.

Each container can now look up the hostname `web` or `db` and get back the appropriate container’s IP address. For example, web's application code could connect to the URL `postgres://db:5432` and start using the Postgres database.

It is important to note the distinction between `HOST_PORT` and `CONTAINER_PORT`.

In the above example, for `db`, the `HOST_PORT` is `8001` and the container port is `5432` (postgres default).

Networked service-to-service communication use the `CONTAINER_PORT`. When `HOST_PORT` is defined, the service is accessible outside the swarm as well.

Within the web container, your connection string to `db` would look like `postgres://db:5432`, and from the host machine, the connection string would look like `postgres://{DOCKER_IP}:8001`.
