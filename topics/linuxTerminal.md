# Terminal vs shell

A shell is a user interface for access to an operating system's services. Most often the user interacts with the shell using a command-line interface (CLI). The terminal is a program that opens a graphical window and lets you interact with the shell.

```
Terminal --> shell --> kernel --> hardware
```

Terminal - It is not a shell, but rather a window serving a shell (xterm, konsole).  
Shell - The actual CLI that executes commands (bash, zsh)

# Locations

```bash
/     # Root directory
~     # Home directory
.     # Current directory
pwd   # Full path to current directory

# WSL / Ubuntu subsystem on Windows
cd /mnt/c     # Navigate to My Computer/C:
# C:\Users\User\AppData\Local\lxss (WSL location in windows)
```

# Commands

```bash
man COMMAND_NAME      # Command manual.

ctrl + c              # Stop running command.
ctrl + d              # Close current shell session i.e. logout.
ctrl + l              # Clear screen. (Scrolls you down in reality)

history               # Lists all the commands used.
ctrl + r              # Search command history. Hit again for previous.
```

# Navigation

```bash
cd FOLDER   # Change directory
cd ..       # Go back one up
cd -        # Go back to last working directory
```

# Files

```bash
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
cp FROM_PATH/FILE TO_PATH/FILE   # Create a copy
mv FROM_PATH/FILE TO_PATH/FILE   # Rename or Cut & Paste a file

# Delete
rm FOLDER/FILE   # Delete folder or file
rm -r FOLDER     # Delete a directory and its files
```

# Find

```bash
grep STRING FILE_NAME           # Find string in specific file.
grep -r STRING .                # Find string in all directory files.

find .                          # Show all files in directory.
find . -name "*.txt"            # Find all text files.
find . -size +5M                # Find files above 5mb.
find . -mtime +3                # Files older than 3 days.

find . -iname FILE_NAME         # Find case insensitive.
find . | grep "string"          # Find files in directory.

which PROGRAM                   # Find path to program.
```

# System

```bash
lsb_release -a      # Linux version.
htop                # Runnig processes. CPU and RAM usage.

date                # Date and time.
timedatectl         # Timezone.
```

# Disk

```bash
df -h --total               # Show disk space in readable format.
du -hx --max-depth=1 .      # Directory disk space usage.
du -ah .                    # Size of all files in location.
```

# Delete

**ALWAYS** select before deleting!!!

```bash
find . -type f -name '*.txt' -mtime +3     # Files older than 3 days.
find . -type f -name '*.txt' -mtime +3 -exec rm {} \;       # DELETE
```

# Install packages

```bash
# apt <--- apt-get <--- dpkg <--- aptitude

apt search PACKAGE              # Search packages.
apt madison PACKAGE             # List versions.

apt update                      # Update the packages list.
apt install PACKAGE             # Install specified package.
apt upgrade                     # Actually update the packages.

apt list --installed            # A list of installed packages.

apt remove PACKAGE              # Remove a specified package.
add-apt-repository REPO         # Add 3rd party repository or PPA (Personal Package Archive).
```

# Download

```bash
curl URL                   # Output the URL content.
wget "URL"                 # Download from URL.
sudo dpkg –i FILE_NAME     # Install downloaded file.
```

# Words

```bash
ctr + ->              # Jump word right.
ctr + <-              # Jump word left.

ctrl + a              # Go to beginning of line.
ctrl + e              # Go to end of line.

ctrl + f              # Next word.
ctrl + b              # Previous word.

alt + backspace       # Delete last word.
alt + left / right    # Go to previous / next word.
```

# Count

```bash
# Number of files
ls | wc -l

# Find all files with the given extensions
# in the specified folders, and count the number of lines.
find folder1 folder2 -name '*.js' -o -name '*.sql' | xargs wc -l
```
