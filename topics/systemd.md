# systemd

This tool is used for managing services (units).  

Service/Daemon is an on-going process that provides a service, usually designed to interact with something outside of it. Ex. web server, monitoring tool, database...  

## systemctl
Manages services.

`systemctl <command> <service>` - Format.

`status` - Shows the status and last lines of log.  
`start` - Starts a service.  
`stop` - Stops a service.

`enable` - Enable start on BOOT.  
`disable` - Disable start on BOOT.

`reload` - Re-read the configuration files.  
`restart` - Restart the service and re-read config files.

## journalctl
Manages logs.

`journalctl -u SERVICE` - Log from a service.  
`journalctl --since "10 min ago"` - Log from last 10 minutes.
