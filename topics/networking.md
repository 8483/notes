# Setup

Tools used for networking.

```bash
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install netcat-openbsd tcpdump traceroute mtr
```

# Theory

Create a basic program that sends a string to the specified ip/port.

Create a second program that listens on that ip/port and prints out any received strings.

Congratulations you now know how every network works. The rest is security, data verification/validation, optimization.

## HTTP / TCP Model

Each of these depends on the one below. Some of these don't belong on a specific layer.

![TEA](../pics/networking/TCPIP.jpg)

HTTP = Hypertext Transfer Protocol  
TCP = Transmission Control Protocol

All traffic is split up into messages called packets, sent between two computers in a stream, with the addresses of the sender and recipient in each one.

HTTP is needed so that communicating systems can understand each other. TCP just provides "envelopes" that can transfer bytes around the network.

An application protocol assigns structure and meaning to the contents of the envelopes.

**If you speak English and I send you a letter written in French, you'll still get the letter, but you won't understand it.**

HTTP is implemented in browsers and web servers, while TCP in the operating system.

#### Simple HTTP Request

```bash
# HTTP header
GET /posts/1 HTTP/1.1
Host: jsonplaceholder.typicode.com

# Manual request - Returns header and body
printf 'GET /posts/1 HTTP/1.1\r\nHost:jsonplaceholder.typicode.com\r\n\r\n' | nc jsonplaceholder.typicode.com 80

# Just the body
curl jsonplaceholder.typicode.com/posts/1
```

## Ports

Listening on a port is like waiting for a phone call on a specific phone number.

Ports let servers distinguish one service from another on the same host and wait for someone to connect.

Normally a server has well known ports for it's applications. ex. HTTP uses `port 80` and SSH uses `port 22`. The client initiates a connection, and it's associated with an arbitrary port on its end.

**Only one program can listen to a port in a given moment**. Once started, the program can start child processes to listen for multiple connections on the same port i.e. a web server.

The port range that a normal (non-root) user can listen on is `1024` through `65535`. Root access (including sudo) can listen on ports down to 1.

If the other side doesn't pick up, an RST (Reset packet) error message is sent back.

# DNS

It's basically a phonebook for IP addresses.

An `A Record` is matched with an IP address, so when someone looks for `www.google.com`, the A record is referenced and the IP address is sent back to the user.

The DNS resolver i.e. client code is built into the OS.

**CNAME** - Canonical Name i.e. alias for a domain.  
**AAAA** - IPv6 equivalent to an A record.  
**NS** - DNS Name server. The NS record for a particular domain specifies which DNS has the records.

# Tools

## ping

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

## lsof

The `lsof` utility lists open files, including network sockets (listening or connected).

```bash
# List only network sockets
lsof -i
```

## nc / netcat

`netcat` is a tool for manually talking to servers, by connecting to a port and sending a string over it. It's a thin wrapper over TCP.

```bash
nc en.wikipedia.org 80
nc localhost 22
nc gmail-smtp-in.l.google.com 80
```

To illustrate, we can use two terminals to talk to each other. Anything typed at the second console will be concatenated to the first, and vice-versa. This is a simple TCP server. The connection is ended with `CTRL` + `d`.

```bash
# terminal 1
nc -l 3456 # listen for an incoming connection on port 3456
# typed text

# terminal 2
nc 127.0.0.1 3456 # connect to the machine and port being listened on
# typed text
```

Commands can be sent via a `pipe`.

```bash
echo 'message' | netcat server 80
```

`netcat` doesn't know anything about forming HTTP request, but in combination with `printf` and `piping`, it can be done.

```bash
printf 'HEAD / HTTP/1.1\r\nHost: google.com\r\n\r\n' | nc google.com 80

printf 'GET /posts/1 HTTP/1.1\r\nHost:jsonplaceholder.typicode.com\r\n\r\n' | nc jsonplaceholder.typicode.com 80
```

## host

Used for looking up records in the DNS.

```bash
# Returns all the records
host google.com

# Returns just the A record
host -t a google.com
```

## dig

Similar to `host` in showing DNS records, but in a way more readable for scripts and closer to the way they are stored in the DNS configuration files.

```bash
dig google.com
```

# Static server

This will start a static web server on port `8000`. The command has to be run in the directory with the `index.html` file.

```bash
python -m SimpleHTTPServer 8080
```

# Networking

**Gateway** is the router address we are talking to in order to connect to the rest of the network/internet.

`127.x.x.x` - Your computer.  
`192.168.0.x` - Local address created by a router.

`ifconfig` - Check IP address.  
`ping 8.8.8.8` - Ping IP address.  
`netstat -tupln` - Check open ports.

`cat etc/network/interfaces` - Shows the interfaces brought up after booting.

`/etc/hosts` is used to simulate a domain for an IP address. Add `127.0.0.1 domain.com` to avoid typing the IP address.

# The following notes are from the book Networking for System Administrators - Michael W. Lucas

# Roles

Every sysadmin, database admin, web admin, developer, and IT professional should understand the basics of networking.

It's **much** easier to teach a system administrator the basics of networking, than to teach a network administrator the basics of system administration.

## System Administrators

Responsible for managing servers i.e. computers, running an OS, whose main task is providing services to other servers or users, rather than the network.

They should:

-   Learn the basics of networking

## Network Administrators

Responsible for managing network equipment, such as routers.

They should:

-   Learn basics of how servers operate.
-   Understand the basics of:
    -   user access control and privileges
    -   processes
    -   services and daemons
    -   install/remove software

# Basic tools

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

# Network Layers

Each layer handles a very specific task and interacts only with layers directly above or below. When a layer breaks, it takes all the above with it. Always try to specify the problematic layer for better support.

-   Textbook 7 layer model - Open Systems Interconnect (OSI)
-   Real-world 5 layer model - Modified TCP/IP
    -   Physical
    -   Datalink
    -   Network
    -   Transport
    -   Application

## 1. Physical layer

This is mostly Ethnernet cables i.e. "The wire". It has no intelligence, as the datalink determines how it's used.

## 2. Datalink layer

Transforms the layers above into signals transmitter over the wire, called `frames`. Most environments use Ethernet as the datalink layer.

IPv4:

-   Media Access Control (MAC)
-   Address Resolution Protocol (ARP)

IPv6:

-   MAC
-   Neighbor Discovery (ND)

## 3. Network layer

Maps connectivity between hosts. i.e. "Can I and how do I get to this other host?"

Provides a consistent interface to network programs.

A single chunk of network data is called a `packet`.

The Internet uses the `Internet Protocol (IP)` which gives each host one or more unique IP addresses, so other hosts can find it.

Network Address Translation (NAT) screws around with the "unique address" rule, but ultimately, you have a globally unique IP address on your or provider's network.

## 4. Transport layer

The data you care about, called `segments`, flows here.

### ICMP - Internet Control Message Protocol

Low-level connectivity messages between hosts i.e. `ping`, re-routing traffic, Datalink layer errors...

### TCP - Transmission Control Protocol

Rich (reliable) transmission of application data between hosts. Has error checking, congestion control, retransmission of lost data.

### UDP - User Diagram Protocol

Simple (unreliable) transmission of application data between hosts. Reliability is handled in the application, rather than layer. Usually video games.

## 5. Application layer

For simplicity and more realistic use, it bundles the actual higher layers:

-   **Session layer** - open/close transport layer connections.
-   **Presentation layer** - Lets programs exchange data.
-   **Application layer** - Actual protocol spoken over these connections.

The `TCP/IP` model calls everything above the transport layer the application layer. It includes protocols like HTTP, SMTP, LDAP...
