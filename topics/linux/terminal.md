# Terminal vs shell

A shell is a user interface for accessing an operating system's services. Most often the user interacts with the shell using a command-line interface (CLI). The terminal is a program that opens a graphical window and lets you interact with the shell.

```
Terminal --> shell --> kernel --> hardware
```

Terminal - It is not a shell, but rather a window interacting with a shell (xterm, konsole).  
Shell - The actual CLI that executes commands (bash, zsh)

# Configuration

```bash
~/.bashrc       # Called every time a terminal is opened.
~/.profile      # Called when the specific user logs in.
/etc/profile    # Called when anyone logs into the system.
```

If your settings are only used in a terminal session then add them to the `~/.bashrc` file as they are only valid during the terminal (bash) session.

But, if you want them to be there with or without a terminal open, add them to the `~/.profile` file. Do not put bash specific commands in the `~/.profile` file.

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

**Default terminal location**

Add this line to the `~/.bashrc` file.

```bash
cd /path/to/desired/directory
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
ls          # Show files
ls -a       # All files, including hidden
ls -l       # In a list format
ls -lh      # In a list format with human readable file sizes

# Read
cat FILE       # Show the file content in terminal
less FILE      # View file content in the less program. Search with / + "text"
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
find                    # Show everything (files and directories).
find /                  # Everything starting from root.
find .                  # Everything in current directory.

find / -name foo        # Show everything name foo starting from root
find . -name foo        # Show everything name foo in current directory.

# Types

find / -name foo -type d           # Show only directories
find / -name foo -type f           # Show only files

# Directories

find ~/app ~/project -name foo     # Searches in multiple directories

# Filtering

find . -name "*.txt"            # Find all text files.
find . -size +5M                # Find files above 5mb.
find . -mtime +3                # Files older than 3 days.

find . -iname FILE_NAME         # Find case insensitive.
find . | grep "string"          # Find files in directory.
```

**Exclude**

`-prune` excludes the directory's contents, but not the directory itself. This happens if `-prune` is the only action in a find command.

If there were any other action (ex. `-exec` or `-print`), it would not output the pruned directory names. So you just have to add an explicit `-print` in the end of your find command.

You also have to add `-o` (OR) to actually print. `-o` is ambiguous, when `find` finds a directory, the `-prune` is true, so `-print` is not evaluated

```bash
# Directories

find ~ -name foo -name "node_modules" -prune -o -name ".git" -prune -o -print

# Files

find ~ -name foo -not -name "*.log" -not -name "*.tmp"

# Both

find ~ -name foo -name "node_modules" -prune -o -name ".git" -prune -o -not -name "*.mp3" -not -name "*.png" -not -name "*.jpg" -not -name "*.svg" -print

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

# Jumping text

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

# Minimize apps

```bash
ctrl + z     # Minimizes the currently open app.
fg           # Opens back the minimized app.
```

# Count

```bash
# Number of files
ls | wc -l

# Find all files with the given extensions
# in the specified folders, and count the number of lines.
find folder1 folder2 -name '*.js' -o -name '*.sql' | xargs wc -l
```
