# Windows Subsystem for Linux (WSL)

Allows using Linux directly in Windows, without double booting or virtual machines.

```bash
# Filesystem
C:\Users\User\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu22.04LTS_79rhkp1fndgsc\LocalState\rootfs

# Terminal .exe
C:\Users\User\AppData\Local\Microsoft\WindowsApps\CanonicalGroupLimited.Ubuntu22.04LTS_79rhkp1fndgsc

# My Computer (C:/D:)
cd /mnt/
```

Install via PowerShell:

```text
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
wsl --set-default-version 2
wsl --install -d Ubuntu-24.04

wsl -l -v

wsl --set-version Ubuntu-24.04 2

lsb_release -a
```

# Command errors

Sometimes commands don't run with `systemctl` i.e. `systemd`...

```
systemctl start nginx

ERROR: System has not been booted with systemd as init system (PID 1). Can't operate. Failed to connect to bus: Host is down
```

...Because in Ubuntu on WSL, many of the common system services still have the "old" `init.d` scripts available to be used in place of `systemctl` with `Systemd` units.

So, for example, you can start `nginx` with `sudo service nginx start`, and it will run the `/etc/init.d/nginx` script with the start argument.

```bash
sudo service nginx start

# The same as doing

/etc/init.d/nginx
```

# Processes

```bash
ps -ef                     # List all running processes.
ps -ef | grep <criteria>   # Find a specific process.
kill -9 <pid>              # Kill a process by id.
```

# Environment Variables

Runtime (temporary) variables for current user, available only during a terminal session.

```bash
env                # List all variables.

export VAR=test    # Create variable.

echo $VAR          # Access variable, method 1.
printenv VAR       # Access variable, method 2.

unset VAR          # Remove variable.
```

To persist them, they need to be declared in the `~/bashrc` file, instead of a terminal. You need to reload the file with `source ~/bashrc`.

To create global variables, for all users, you do the same in `/etc/environment`.

# $PATH

`$PATH` is an enviroment variable which includes a list of important directories that include program executables i.e. allows to run commands.

Each time you run a command, the shell checks all the directories for the executable.

You can also check if an execuatable is in the path.

```bash
which executable_name
```

Directories that include executables are usually named `bin`.

```bash
# So you can run programs like this...
vim file.txt

# Instead of...
/usr/bin/vim file.txt

# Location of a command, ex. ls
which ls # /usr/bin/ls
```

The directories are separated by a colon `:`.

```bash
echo $PATH

/home/name/.nvm/versions/node/v20.13.1/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/mnt/c/Program Files/WindowsApps/CanonicalGroupLimited.Ubuntu22.04LTS_2204.3.63.0_x64__79rhkp1fndgsc:/mnt/c/Program Files/AdoptOpenJDK/jre-11.0.7.10-hotspot/bin:/mnt/c/Program Files/AdoptOpenJDK/jre-8.0.252.09-hotspot/bin:/mnt/c/Program Files (x86)/AdoptOpenJDK/jre-8.0.252.09-hotspot/bin:/mnt/c/WINDOWS/system32:/mnt/c/WINDOWS:/mnt/c/WINDOWS/System32/Wbem:/mnt/c/WINDOWS/System32/WindowsPowerShell/v1.0/:/mnt/c/WINDOWS/System32/OpenSSH/:/mnt/c/Program Files (x86)/Microsoft SQL Server/150/DTS/Binn/:/mnt/c/Program Files/dotnet/:/mnt/c/Users/Name/AppData/Local/Microsoft/WindowsApps:/mnt/c/Program Files (x86)/Nmap:/mnt/c/Users/Name/AppData/Local/Programs/Microsoft VS Code/bin:/snap/bin
```

### Add directories

```bash
# Add the ~/bin i.e. /home/user/bin directory to PATH
PATH=$PATH:~/bin
```

`$PATH` returns to normal i.e. everything is lost when the terminal is closed. We need to persist it to keep the modifications.

### Persist directories

Add paths to `~/.profile` to make them permanent. Paths require the `bin`, while variables don't.

**The variable is not in the environment until you export it. Otherwise it's just a shell variable.**

```bash
# Method 1 ---------------------

# Edit this file.
vim ~/.profile

# Find this line.
PATH=$HOME/bin:$PATH

# Change it into this.
PATH=$HOME/bin:$PATH:/usr/hitech/picc/9.82/bin

# It appends the new path to the existing system ones, just for this user.

# Method 2 ---------------------

# Shorthand to avoid editing manually.
export PATH=$PATH:/usr/hitech/picc/9.82/bin
```

Reload with

```bash
source ~/.profile
```
