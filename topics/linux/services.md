# service

`service` is an "high-level" command used for starting and stopping services in different unixes and linuxes.

```bash
sudo service <service> <command>

# Commands
status        # Shows the status and last lines of log.
start         # Starts a service.
stop          # Stops a service.

reload        # Re-read the configuration files.
restart       # Restart the service and re-read config files.

# List
service --status-all
```

Depending on the "lower-level" service manager, service redirects to different binaries:

-   For CentOS 7 it redirects to `systemctl`.
-   For CentOS 6 it directly calls the relative `/etc/init.d` script.
-   In older Ubuntu releases it redirects to `upstart`.

`service` is adequate for basic service management, while directly calling `systemctl` give greater control options.

# systemctl

`systemctl` is a command-line tool that interacts with `systemd`. It is used to control the `systemd` system and service manager.

```bash
sudo systemctl <command> <service>

# Commands
enable        # Enable start on BOOT.
disable       # Disable start on BOOT.

# List
systemctl list-units
```

# systemd

> `systemd` handles initializing everything that needs to launch behind the scenes when starting up Linux.

Anything that ends in a "d" is a daemon i.e. a process that works in the background.

Systemd is the daddy of processes. Process ID number 1. It starts all other processes/daemons at boot. It has integrated tools that manage, for example, wifi, bluetooth, suspend/shutdown, etc.

The original way of starting everything when a linux system boots was to just run each thing, one after another with scripts.

Systemd started as a more intelligent way of doing things, which could handle starting multiple things at the same time and thought about dependencies - you can't start things that need the network until you've started the network devices, for example. It mostly seemed like a good idea.

Systemd has since grown considerably, taking over other tasks. This has been controversial. Some people feel that systemd is forcing its way into things that an init system has no business touching.

So, at it's core Systemd is an init system, which is the first program that runs at boot, it launches all your daemons, and just performs general boot processes. People dislike it because it is getting pretty large, and some worry that it will become the next Xorg, in that it will become a chore to maintain, and it would be difficult to audit, along with Lenart Poeterring (creator of Systemd) being known for dismissing some potential problems.

This tool is used for managing services (units).

Service/Daemon is an on-going process that provides a service, usually designed to interact with something outside of it. Ex. web server, monitoring tool, database...

# journalctl

Manages logs.

```bash
journalctl -u SERVICE              # Log from a service.
journalctl --since "10 min ago"    # Log from last 10 minutes.
```
