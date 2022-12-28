# TCP - Transmission Control Protocol

TCP is implemented in the operating system.

All traffic is split up into messages called packets, sent between two computers in a stream, with the addresses of the sender and recipient in each one.

TCP just provides "envelopes" that can transfer bytes around the network.

An application protocol assigns structure and meaning to the contents of the envelopes.

**If you speak English and I send you a letter written in French, you'll still get the letter, but you won't understand it.**

# Ports

IP addresses locate devices. Ports locate programs on the devices.

```bash
netstat -n

Proto   Local addresses    Foreign addresses   State

TCP    92.168.0.12:52913   215.114.85.17:80    ESTABLISHED # HTTP Web server
TCP    92.168.0.12:52920   215.114.85.17:21    ESTABLISHED # FTP server
```

Ports are like "IP addresses" for programs/services we want to use i.e. locating where they are running on a device, which has an actual IP address to locate it.

| Location |       Type        |      Range      |                           Example                           |
| :------: | :---------------: | :-------------: | :---------------------------------------------------------: |
|  Server  | System/Well-known |    0 - 1,023    |          80 (HTTP), 443 (SSL), 25 (SMTP), 21 (FTP)          |
|  Server  |  User/Registered  | 1,024 - 49,151  |          1433 (MSSQL), 1527 (Oracle), 1102 (Adobe)          |
|  Client  |  Dynamic/Private  | 49,152 - 65,535 | Termporarily assigned for client while using above services |

Listening on a port is like waiting for a phone call on a specific phone number.

Ports let servers distinguish one service from another on the same host and wait for someone to connect.

Ports are not physical, but logical.

Ports are always associated with an IP address, allowing programs to exchange data over the network.

Normally a server has well known ports for it's applications. The client initiates a connection, and it's associated with an arbitrary port on its end.

**Only one program can listen to a port in a given moment**. Once started, the program can start child processes to listen for multiple connections on the same port i.e. a web server.

The port range that a normal (non-root) user can listen on is `1024 - 65535`. Root access (including sudo) can listen on ports down to 1.

If the other side doesn't pick up, an RST (Reset packet) error message is sent back.
