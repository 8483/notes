# Vagrant

Vagrant is used to avoid the tedious provisioning of Virtual Machines. Instead of manually installing the OS for every machine, VMs can be spun up instantly by downloading an image (box) from the hub, and running it.  

Vagrant **is not** like Docker. Vagrant is used to start VMs fast, while Docker is used to containerize packages within the VMs. Vagrant avoids installing an OS, Docker avoids installing packages i.e. they just run them. This significantly speeds things up by allowing to quickly create and destroy whole environments.  

In other words, use Vagrant for just the OS and Docker install, and run everything else as a Docker container.  

**Host** = Main Machine. **Guest** = Virtual Machine.  

## Install
`wget "URL"` - Download Vagrant.   
`sudo dpkg -i FILENAME` - Install Vagrant.  
`sudo apt-get install virtualbox` - Download VirtualBox.

## Virtual Machine
`vagrant box add BOX_NAME` - Download a box.  
`vagrant init BOX_NAME` - Create Vagrantfile i.e. VMs live here.   
`vagrant up` - Start a box (VM).
