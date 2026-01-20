# Processes

```bash
ps -ef                     # List all running processes.
ps aux                     # All running processes with details.

ps -ef | grep <criteria>   # Find a specific process.

kill -9 <pid>              # Kill a process by id.
```

Output columns:

- `USER` – process owner
- `PID` – process ID
- `%`CPU – CPU usage
- `%`MEM – memory usage
- `VSZ` – virtual memory size
- `RSS` – resident memory size
- `TTY` – terminal associated
- `STAT` – process state
- `START` – start time
- `COMMAND` – command that launched it

# Errors

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
