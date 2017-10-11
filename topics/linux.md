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
`ssh user@SERVER-IP-ADDRESS` - SSH to server.  
`ssh -p PORT user@SERVER-IP-ADDRESS` - Via port.  

## Notes
`dpkg` is a backend for `apt-get` which is a backend for `aptitude` (GUI).
