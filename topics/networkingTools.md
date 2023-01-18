Tools used for networking.

```bash
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install netcat-openbsd tcpdump traceroute mtr
```

| unix             | windows  | use                                                         |
| ---------------- | -------- | ----------------------------------------------------------- |
| ifconfig / route | ifconfig | View the system's network configuration                     |
| grep             | findstr  | Search for specific text                                    |
| netstat          | netstat  | Display established network connections and statistics      |
| lsof             |          | What processes open which files                             |
| route            |          | Display where and change how traffic is sent                |
| tcpdump          |          | Display traffic to and from a server, view network activity |
| traceroute       | tracert  | Show the route the traffic takes                            |
| host             | nslookup | Explore the Domain Name System (DNS)                        |

# Public (WAN) IP address

```
curl ipinfo.io/ip
curl ipecho.net/plain
curl ifconfig.me
curl icanhazip.com
```

# Private/Local (LAN) IP address

PC's don't have IP addresses. Network interfaces have them, meaning one PC will have multiple IP addresses.

1. Find IP address of PC.

```bash
ip addr
ip -c addr
ip -o -c addr

# IP address: 192.168.100.16/24
# /24 = 255.255.255.0 subnet mask
```

2. Scan subnet for other devices (IP addresses).

```bash
nmap -sn 192.168.100.0/24
```

3. Ping other device.

```bash
ping 192.168.100.10
```

4.  Default gateway (router)

```
ip route show
route -n
netstat -rn
```

# Interfaces

```
ls /sys/class/net/
netstat -i
ip link show
```

# ping

It sends individual packets to test if traffic can get from one address to another, and back.

```bash
ping 8.8.8.8

# send 5 packets
ping -c 5 google.com
```

#### unknown host

```bash
sudo vim /etc/resolv.conf
# Add nameserver 8.8.8.8
```

# lsof

The `lsof` utility lists open files, including network sockets (listening or connected).

```bash
# List only network sockets
lsof -i
```

# nc / netcat

`netcat` is a tool for manually talking to servers, by connecting to a port and sending a string over it. It's a thin wrapper over TCP.

```bash
nc en.wikipedia.org 80
nc localhost 22
nc gmail-smtp-in.l.google.com 80
```

To illustrate, we can use two terminals to talk to each other. This is a simple TCP server.

**Anything typed at the second console will be concatenated to the first, and vice-versa.**

The connection is closed with `CTRL` + `d`.

```bash
# terminal 1 - Listen for an incoming connection on port 6666
nc -l 6666
# typed: foo
# shown: bar

# terminal 2 - Connect to the machine on that listening port
nc 127.0.0.1 6666
# show: foo
# typed: bar
```

Commands can be sent via a `pipe`.

```bash
echo 'message' | netcat server 80
```

`netcat` doesn't know anything about forming HTTP request, but in combination with `printf` and `piping`, it can be done.

```bash
# Google
printf 'HEAD / HTTP/1.1\r\nHost: google.com\r\n\r\n' | nc google.com 80

# JSON
printf 'GET /posts/1 HTTP/1.1\r\nHost:jsonplaceholder.typicode.com\r\n\r\n' | nc jsonplaceholder.typicode.com 80
```

# host

Used for looking up records in the DNS.

```bash
# Returns all the records
host google.com

# Returns just the A record
host -t a google.com
```

# dig

Similar to `host` in showing DNS records, but in a way more readable for scripts and closer to the way they are stored in the DNS configuration files.

```bash
dig google.com
```

# Static server

This will start a static web server on port `8000`. The command has to be run in the directory with the `index.html` file.

```bash
python -m SimpleHTTPServer 8080
```

`ifconfig` - Check IP address.  
`ping 8.8.8.8` - Ping IP address.  
`netstat -tupln` - Check open ports.

`cat etc/network/interfaces` - Shows the interfaces brought up after booting.

`/etc/hosts` is used to simulate a domain for an IP address. Add `127.0.0.1 domain.com` to avoid typing the IP address.

# tshark

The terminal version of Wireshark, used for packet sniffing.

# traceroute

The asterisks you're seeing are servers that your packets are being routed through whom are timing out (5.0+ seconds) and so traceroute defaults to printing the \*.
