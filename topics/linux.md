# Ubuntu on Windows - WSL

`C:\Users\User\AppData\Local\lxss\home\user` - Filesystem location.

`cd /mnt/` - Navigate to My Computer (C:, D:).

Run commands without `systemctl` i.e. `systemd`...

```
systemctl start nginx

ERROR: System has not been booted with systemd as init system (PID 1). Can't operate. Failed to connect to bus: Host is down
```

```bash
# In Ubuntu on WSL, many of the common system services still have the "old" init.d scripts available to be used in place of systemctl with Systemd units.

ls /etc/init.d/

# So, for example, you can start nginx with sudo service nginx start, and it will run the /etc/init.d/nginx script with the start argument.

sudo service nginx start
```

# Processes

```bash
ps -ef                     # List all running processes.
ps -ef | grep <criteria>   # Find a specific process.
kill -9 <pid>              # Kill a process by id.
```

# Environment Variables

PATH is an enviroment variable. It tells your machine where to search for program executables, so when you run your **picc** program you can just do `picc` instead of `/usr/hitech/picc/9.82/bin/picc`.

```bash
env            # List all variables.
echo "$HOME"   # Specific variable, in this case **$PATH**.
```

Add paths to `~/.profile` to make them permanent. Paths require the `bin`, while variables don't.

**The variable is not in the environment until you export it. Otherwise it's just a shell variable.**

Paths are delimited with a colon `:`

```bash
# Method 1
vim ~/.profile # Edit this file.
PATH="$HOME/bin:$PATH" # Find this line.
PATH="$HOME/bin:$PATH:/usr/hitech/picc/9.82/bin" # Change it into this.
# It appends the new path to the existing system ones, just for this user.

# Method 2 - Shorthand to avoid editing manually.
export PATH="$PATH:/usr/hitech/picc/9.82/bin" # export <VARIABLE>="<VALUE>"
```
