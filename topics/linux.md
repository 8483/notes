# Linux

#### Help
`man COMMAND` - Command help.

#### Install
`apt-get install PACKAGE` - Install specified package.  
`apt-cache search NAME` - Search packages.  

`wget "URL"` - Download from URL.    
`sudo dpkg –i FILE_NAME` - Install downloaded file.  

#### Folders & Files
`mkdir FOLDER` - Create a folder.   
`touch FILE` - Create a file.

`rm FOLDER/FILE` - Delete folder or file.  
`rm -r FOLDER` - Delete a directory and its files.

`cp PATH/FILE PATH/FILE` - Create a copy.  
`mv PATH/FILE PATH/FILE` - Rename or Cut & Paste a file.

#### SSH
`sudo apt-get install openssh-server` - Install SSH.  
`ssh user@SERVER-IP-ADDRESS` - SSH to server.  
`ssh -p PORT user@SERVER-IP-ADDRESS` - Via port.  

SSH in Virtual Machine. Needs a port forwarding rule in network settings for the VM. Name `ssh`, host port `3022`, guest port `22`.

`ssh -p 3022 user@127.0.0.1` - SSH to VM locally.  

## Notes
`dpkg` is a backend for `apt-get` which is a backend for `aptitude` (GUI).
