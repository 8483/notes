# Linux

# Basics

`/` - Root directory  
`~` - Home directory

`^` - Control key.  
`M` - Alt key.

`CTRL` + `C` - Stop running command.  
`CTRL` + `D` - Close current shell session.  
`CTRL` + `L` - Clear screen. (Scrolls you down in reality)

`CTRL` + `A` - Go to beginning of line.  
`CTRL` + `E` - Go to end of line.  
`CTRL` + `F` - Next word.  
`CTRL` + `B` - Previous word.

`ALT` + `Backspace` - Delete last word.  
`ALT` + `Left` / `Right` - Go to previous / next word.  
`CTRL` + `U` - Delete whole line.

`man COMMAND` - Command help.  
`history` - Lists all the commands used.  
`CTRL` + `R` - Search command history. Hit again for previous command.

# Install

`dpkg` is a backend for `apt-get`, which is a backend for `aptitude` (GUI).

## Packages

```bash
apt-cache search PACKAGE   # Search packages.
apt-cache madison PACKAGE  # List versions.

apt-get update             # Update the packages list.
apt-get install PACKAGE    # Install specified package.
apt-get upgrade            # Actually update the packages.

apt list --installed       # A list of installed packages.

apt-get remove PACKAGE     # Remove a specified package.
add-apt-repository REPO    # Add 3rd party repository or PPA (Personal Package Archive).

curl URL                   # Output the URL content.
wget "URL"                 # Download from URL.
sudo dpkg –i FILE_NAME     # Install downloaded file.
```

## .deb

Install `.deb` packages from the terminal.

```bash
sudo dpkg -i <path/to/deb.deb>
sudo apt-get install -f
```

# Find

```bash
find /                                    # List root directory's content.
find / | grep FILE                        # Search the output.

sudo find / -iname FOLDER/FILE.ext    # Find case insensitive.
which PROGRAM                         # Find path to program.
```

# Count
```bash
ls | wc -l                                # Number of files
find ./folder -name '*.js' | xargs wc -l  # Lines of code per file type
```


# Utility

```bash
# Navigation
cd FOLDER   # Change directory
cd ..       # Go back one up
cd -        # Go back to last working directory
pwd         # Current path

# List
ls -a       # List all files, including hidden
ls -l       # List in a list format
ll          # Shorthand for ls -l
ls -lh      # Show file size

# Read
cat FILE       # Show the file content in terminal
less FILE      # View file content in the less program
head FILE      # Show first 10 lines
tail FILE      # Show last 10 lines
tail -f FILE   # Log in real time

# Create
mkdir FOLDER   # Create a folder
touch FILE     # Create a file

# Copy/Paste
cp PATH/FILE PATH/FILE   # Create a copy
mv PATH/FILE PATH/FILE   # Rename or Cut & Paste a file

# Delete
rm FOLDER/FILE   # Delete folder or file
rm -r FOLDER     # Delete a directory and its files

# Printing
echo TEXT      # print text
printf TEXT    # print formatted text
```

```bash
\n   # new line
\r   # carriage return, i.e. bring carret (cursor) to start of line
```

# Processes

```bash
ps -ef                     # List all running processes.
ps -ef | grep <criteria>   # Find a specific process.
kill -9 <pid>              # Kill a process by id.
```

# Environment Variables

PATH is an enviroment variable. It tells your machine where to search for program executables, so when you run your **picc** program you can just do `picc` instead of `/usr/hitech/picc/9.82/bin/picc`.

Add them to `~/.profile` to make them permanent. Paths require the `bin`, while variables don't.

The variable is not in the environment until you export it. Otherwise it's just a shell variable.

```bash
env            # List all variables.
echo "$HOME"   # Specific variable, in this case **$PATH**.
```

Paths are delimited with `:`.

```bash
# Method 1
vim ~/.profile # Edit this file.
PATH="$HOME/bin:$PATH" # Find this line.
PATH="$HOME/bin:$PATH:/usr/hitech/picc/9.82/bin" # Change it into this.
# It appends the new path to the existing system ones, just for this user.

# Method 2 - Shorthand to avoid editing manually.
export PATH="$PATH:/usr/hitech/picc/9.82/bin" # export <VARIABLE>="<VALUE>"
```

# Password Generator

Generate a random 14 character password by using the linux `/dev/urandom` file, a stream of mashed system data.

`cat /dev/urandom | env LC_CTYPE=C tr -dc a-zA-Z0-9 | head -c 14`

# Ubuntu on Windows

`C:\Users\User\AppData\Local\lxss\home\user` - Filesystem location.

`cd /mnt/` - Navigate to My Computer (C:, D:).

# How to Compile and Run a C Program

```bash
# Create file
touch hello.c

# Edit file
vim hello.c
```
Paste this C code in the file.
```c
#include <stdio.h>

main() {
    printf("Hello World\n");
}
```

``` bash
# Compile the code into hello program
gcc -o hello hello.c

# Run the program
./hello
```