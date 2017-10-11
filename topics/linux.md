# Linux

Plain text file > Compilation > Binary (.exe) i.e. package. Ubuntu is simply a collection (repository) of pre-compiled packages, running over a kernel (Big pile of software that knows how to make the hardware do stuff).  

**"On a UNIX system, everything is a file; if something is not a file, it is a process."** (directory is a file containing names of other files)  

File extensions are meaningless. They are there for the user's sake.  

`tree DIRECTORY` - Show directory tree. `tree .` for current.  

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
`adduser USERNAME` - Guided user creation.  
`useradd --uid <UID> --gid <GID> -m -s /usr/bin/zsh -d /var/www/USERNAME --password PASSWORD USERNAME` - Scriptable usere creation.  

`su - USERNAME` - Log in as a user.  
`CTRL` + `d` - Logout.

## Permissions
`ls -l` - Show file permissions.  

`chown USER:GROUP FILENAME` - Change file owner.  

`chmod` - Change file mode i.e. `rwx` permissions.  

`r` - Read.  
`w` - Write.  
`x` - Execute.  

Permission | File Type | Owner | Group | Other | Meaning
:---: | :---: | :---: | :---: | :---: | :---:
-rwx------ | - | rwx | --- | --- | File that only the owner can read, write and execute.
drw-rw---- | d | rw- | rw- | r-- | Directory that owner and group can read/write, others just read

Owner permissions trump group ones.  

`chmod +x FILE` - Add execute permission to owner, group and others.  
`chmod u+w FILE` - Add write permission to just the owner.  
`chmod o-r FILE` - Remove read permission to other users.  

This can get very tedious for each permission, so instead, we can set the permissions by using octal numbers.  

First number is **owner**, second is **group**, and third is **others**.

`chmod 644 FILE` - Owner read/write, group read, others read.  

Dec | Permission
:---: | ---
7 | Read, write, execute
6 | Read, write
5 | Read, execute
4 | Read
3 | Write, execute
2 | Write
1 | Execute
0 | No permissions

`7, 6, 5, 4, 0` - Most used.  

A binary mask is used to determine the numbers. Add the column values for each 1 and the sum is the decimal number.  

 | Read (4) | Write (2) | Execute (1) | Decimal
 :---: | :---: | :---: | :---: | :---: 
**User** | 1 | 1 | 1 | 7
**Group** | 1 | 0 | 1 | 5
**Other** | 1 | 0 | 0 | 4

The number `754` would give the owner full permissions, the group read/execute, and just read for the others.  

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
