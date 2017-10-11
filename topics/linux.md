# Linux

Plain text file > Compilation > Binary (.exe) i.e. package. Ubuntu is simply a collection (repository) of pre-compiled packages, running over a kernel (Big pile of software that knows how to make the hardware do stuff).  

**"On a UNIX system, everything is a file; if something is not a file, it is a process."** (directory is a file containing names of other files)  

File extensions are meaningless. They are there for the user's sake.  

## Basics
`^` - Control key.  
`M` - Alt key.  

`CTRL + a` - Go to beginning of line.  
`CTRL + e` - Go to end of line.  

## Help
`man COMMAND` - Command help.

## Install
`apt-get install PACKAGE` - Install specified package.  
`apt-cache search NAME` - Search packages.  
`apt-get update` - Update the packages list.  
`apt-get upgrade` - Actually update the packages.  
`apt-get remove PACKAGE` - Remove a specified package.  
`add-apt-repository REPO` - Add 3rd party repository or PPA (Personal Package Archive).  

`wget "URL"` - Download from URL.    
`sudo dpkg –i FILE_NAME` - Install downloaded file.  

## Find
`sudo find / -iname FOLDER/FILE.ext` - Find case insensitive.  
`which PROGRAM` - Find path to program.

## Folders & Files

#### List
`ls -a` - List all files, including hidden.  
`cat FILE` - Show the file's content.  

#### Create
`mkdir FOLDER` - Create a folder.   
`touch FILE` - Create a file.

#### Copy/Paste
`cp PATH/FILE PATH/FILE` - Create a copy.  
`mv PATH/FILE PATH/FILE` - Rename or Cut & Paste a file.

#### Delete
`rm FOLDER/FILE` - Delete folder or file.  
`rm -r FOLDER` - Delete a directory and its files.

## SSH
`sudo apt-get install openssh-server` - Install SSH.  

`ssh user@SERVER-IP-ADDRESS` - SSH to server.  
`ssh -p PORT user@SERVER-IP-ADDRESS` - Via port.  

SSH in Virtual Machine needs a port forwarding rule in network settings for the VM. Name `ssh`, host port `3022`, guest port `22`.

`ssh -p 3022 user@127.0.0.1` - SSH to VM locally.  

## Configuration

All config files are located in `/etc/`. They are plain text files.  

Use the `less` program to view configuration files, instead of an editor. Ex. `less /etc/ssh/sshd_config`.  

`/`+`string` - Search inside **less** (case sensitive).  
`q` - Exit.

## Users & Groups



## Networking
**Gateway** is the router address we are talking to in order to connect to the rest of the network/internet.  

`127.0.0.1` - Your computer.  
`192.168.x.x` - Local address created by a router.  

`ifconfig` - Check IP address.  
`ping 8.8.8.8` - Ping IP address.  
`netstat -tupln` - Check open ports.

`cat etc/network/interfaces` - Shows the interfaces brought up after booting.  

## Notes
`dpkg` is a backend for `apt-get` which is a backend for `aptitude` (GUI).
