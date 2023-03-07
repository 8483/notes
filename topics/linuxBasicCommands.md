# Terminal

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

# Information

```bash
cat /etc/os-release     # Linux version
df -h --total           # Show disk space in readable format
htop                    # CPU and RAM usage
```

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
grep -r 'string' directory_to_search      # List occurences of string in all files.
find /                                    # List root directory's content.
find / | grep FILE                        # Search the output.

sudo find / -iname FOLDER/FILE.ext    # Find case insensitive.
which PROGRAM                         # Find path to program.
```

# Count

```bash
# Number of files
ls | wc -l

# Find all files with the given extensions
# in the specified folders, and count the number of lines.
find folder1 folder2 -name '*.js' -o -name '*.sql' | xargs wc -l
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

```bash
# Compile the code into hello program
gcc -o hello hello.c

# Run the program
./hello
```

# Password Generator

Generate a random 14 character password by using the linux `/dev/urandom` file, a stream of mashed system data.

`cat /dev/urandom | env LC_CTYPE=C tr -dc a-zA-Z0-9 | head -c 14`

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

```bash
# Compile the code into hello program
gcc -o hello hello.c

# Run the program
./hello
```
