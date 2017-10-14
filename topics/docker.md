# Docker

Docker is used to run apps/processes without actually installing them. This is useful because it ensures the same environment is running on all the machines using the images that make the containers.  

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
`-d` - Run as daemon i.e. background process.  
`-p 8080:80` - Map the container's `80` port, to the webserver's `8080` port i.e. the container can be accessed via `8080`.   
