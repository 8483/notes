# systemd

The original way of starting everything when a linux system boots was to just run each thing, one after another with scripts.

Systemd started as a more intelligent way of doing things, which could handle starting multiple things at the same time and thought about dependencies - you can't start things that need the network until you've started the network devices, for example. It mostly seemed like a good idea.

Systemd has since grown considerably, taking over other tasks. This has been controversial. Some people feel that systemd is forcing its way into things that an init system has no business touching.

First, anything that ends in a "d" is a daemon. That means is a process that works in the background.

Systemd it's the daddy of processes. Process ID number 1. It starts all other processes/daemons at boot. It has integrated tools that manage, for example, wifi, bluetooth, suspend/shutdown, etc.

So, at it's core Systemd is an init system, which is the first program that runs at boot, it launches all your daemons, and just performs general boot processes. People dislike it because it is getting pretty large, and some worry that it will become the next Xorg, in that it will become a chore to maintain, and it would be difficult to audit, along with Lenart Poeterring (creator of Systemd) being known for dismissing some potential problems.

This tool is used for managing services (units).

Service/Daemon is an on-going process that provides a service, usually designed to interact with something outside of it. Ex. web server, monitoring tool, database...

# systemctl

Manages services.

```bash
systemctl <command> <service>
              ^
              |
            status    # Shows the status and last lines of log.
            start     # Starts a service.
            stop      # Stops a service.

            enable    # Enable start on BOOT.
            disable   # Disable start on BOOT.

            reload    # Re-read the configuration files.
            restart   # Restart the service and re-read config files.
```

# journalctl

Manages logs.

```bash
journalctl -u SERVICE              # Log from a service.
journalctl --since "10 min ago"    # Log from last 10 minutes.
```
