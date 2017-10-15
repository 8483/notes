# Docker

Docker is used to run apps/processes without actually installing them. This is useful because it ensures the same environment is running on all the machines using the images that make the containers.  

**Container** is a running instance of an **image**, which is a template for an environment, a snapshot of a system at a certain time. It has the OS, software and application code all bundled in one file. Images are built via a **Dockerfile**, a list of steps to perform to build the image. List of steps like configure the OS, install packages, copy project files into the right places...  

Dockerfile > (build) Image > (run) Container

Docker uses a `unix socket` which means that it runs as `root` and asks for `sudo`.  

## Installing Docker

Add the GPG key.  
`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

Add the Docker repository to APT sources.  
`sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

Update the package list to include Docker.  
`sudo apt-get update`

Install Docker.  
`sudo apt-get install -y docker-ce`

## Basic Commands

`docker run <FLAGS> <IMAGE>` - Build a container from an image and run it. Each usage creates a new container.  
`docker start <NAME/ID>` - Start an existing container by name or id.  
`docker stop <NAME/ID>` - Stop a container by name or id.  
`docker ps` - List all running containers. `-a` for stopped ones.  
`docker rm <NAME/ID>` - Delete a container by name or id.    

## Flags  

`--name <NAME>` - Name the container.  
`-p 8080:80` - Map the container's `80` port, to the webserver's `8080` port i.e. the container can be accessed via `8080`.  
`-d` - Run as daemon i.e. background process.  
`-v <SOURCE>:<DESTINATION>` - Volume. Mount a local directory as a container directory. Used when not specified in Dockerfile.  

## Build Container with Dockerfile

#### Dockerfile
`FROM <IMAGE>` - Always at top. Uses an image as a base.    
`RUN <COMMAND>` - Run any bash commands. Can be chained with &&.  
`COPY <SOURCE> <DESTINATION>` - Copy from local directory to container directory.  
`EXPOSE port` - Expose a specific port.  
`#` - Comment.  

#### Build
`docker build -t <NAME> <SOURCE>` - Build an image by running all commands in the Dockerfile.  

###### nginx Container Example
```Dockerfile
FROM nginx
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
COPY /src /www
```

```
# ./nginx.conf - Bare minimum needed to run.
server {
    root /www;
}
```

`sudo docker build -t nginx-static .` - Build the container.  

`sudo docker run -d -p 8080:80 --name nginx-static nginx-static` - For this to work, a port forwarding rule is needed on the VM with host `3000` and guest `8080`. Container `80` > VM `8080` > Host `3000`.  

Visit the website in the host via `127.0.0.1:3000`.
