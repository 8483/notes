# Linux

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

`curl URL` - Output the URL content.  
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

## Networking
**Gateway** is the router address we are talking to in order to connect to the rest of the network/internet.  

`127.0.0.1` - Your computer.  
`192.168.x.x` - Local address created by a router.  

`ifconfig` - Check IP address.  
`ping 8.8.8.8` - Ping IP address.  
`netstat -tupln` - Check open ports.

`cat etc/network/interfaces` - Shows the interfaces brought up after booting.  

`/etc/hosts` is used to simulate a domain for an IP address. Add `127.0.0.1 domain.com` to avoid typing the IP address.  

## Notes
`dpkg` is a backend for `apt-get` which is a backend for `aptitude` (GUI).

## Password Generator

Generate a random 14 character password by using the linux `/dev/urandom` file, a stream of mashed system data.  

`cat /dev/urandom | env LC_CTYPE=C tr -dc a-zA-Z0-9 | head -c 14`
