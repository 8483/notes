# Terminal vs shell

A shell is a user interface for access to an operating system's services. Most often the user interacts with the shell using a command-line interface (CLI). The terminal is a program that opens a graphical window and lets you interact with the shell.

```
Terminal --> shell --> kernel --> hardware
```

Terminal - It is not a shell, but rather a window serving a shell. (xterm, konsole)

Shell - The actual CLI that executes commands. (bash, zsh)

# Locations

```bash
/     # Root directory
~     # Home directory

# WSL / Ubuntu subsystem on Windows
cd /mnt/c     # Navigate to My Computer/C:.
# C:\Users\User\AppData\Local\lxss (WSL location in windows)
```

# Terminal

```bash
ctrl + c              # Stop running command.
ctrl + d              # Close current shell session i.e. logout.
ctrl + l              # Clear screen. (Scrolls you down in reality)
```

# Commands

```bash
man COMMAND           # Command help.

ctrl + a              # Go to beginning of line.
ctrl + e              # Go to end of line.
ctrl + f              # Next word.
ctrl + b              # Previous word.

alt + backspace       # Delete last word.
alt + left / right    # Go to previous / next word.
ctrl + u              # Delete whole line.

history               # Lists all the commands used.
ctrl + r              # Command history. Hit again for previous.

echo TEXT             # print text
printf TEXT           # print formatted text
```

# Navigation

```bash
cd FOLDER   # Change directory
cd ..       # Go back one up
cd -        # Go back to last working directory

ls -a       # List all files, including hidden
ls -l       # List in a list format
ll          # Shorthand for ls -l
ls -lh      # Show file size

pwd         # Current path
```

# Files

```bash
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
```

# Find

```bash
grep -r 'string' directory_to_search      # List occurences of string in all files.
find /                                    # List root directory's content.
find / | grep FILE                        # Search the output.
find / -size +5M -ls                      # Find files above 5mb.

sudo find / -iname FOLDER/FILE.ext        # Find case insensitive.
which PROGRAM                             # Find path to program.
```

# System

```bash
lsb_release -a      # Linux version.
htop                # Runnig processe. CPU and RAM usage.
```

# Disk

```bash
df -h --total               # Show disk space in readable format.
du -hx --max-depth=1 .      # Directory disk space usage
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

# Count

```bash
# Number of files
ls | wc -l

# Find all files with the given extensions
# in the specified folders, and count the number of lines.
find folder1 folder2 -name '*.js' -o -name '*.sql' | xargs wc -l
```
