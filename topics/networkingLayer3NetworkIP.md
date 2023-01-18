# Layer 3 - Network

1. We want to transfer data from one IP address to another.
2. First device emits a broadcast only the subnet can see, i.e. local network.
3. IF device is not there, the data is forwarded to the router.
4. Routers pass on the data until the matching subnet is reached.
5. Data enters that network and goes to the desired IP address in that local network.

# Public (WAN) vs Private (LAN)

You can't access the internet with a private (local) IP address.

The conversion from private (local) to public IP is done by NAT (Network Address Translation)

|  Public (WAN)   |                Private/Local (LAN)                |
| :-------------: | :-----------------------------------------------: |
|     Unique      | Not unique, can be used on other private networks |
| Used externally |                  Used internally                  |
| Assigned by ISP |       Assigned by the router's DHCP service       |

# IP Address

A numeric identifier for a computer or device on a network.

It's composed of 4 octets in the 0-255 range.

```
[0-255].[0-255].[0-255].[0-255]

66.94.234.13
```

Computers read IP addresses in a binary format.

```
66.94.234.13 = 01000010.01011110.00011101.00001101
```

| Octet |         Sum         | 128 | 64  | 32  | 16  |  8  |  4  |  2  |  1  |
| :---: | :-----------------: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
|  66   |       64 + 2        |  0  |  1  |  0  |  0  |  0  |  0  |  1  |  0  |
|  94   | 64 + 16 + 8 + 4 + 2 |  0  |  1  |  0  |  1  |  1  |  1  |  1  |  0  |
|  234  |   16 + 8 + 4 + 1    |  0  |  0  |  0  |  1  |  1  |  1  |  0  |  1  |
|  13   |      8 + 4 + 1      |  0  |  0  |  0  |  0  |  1  |  1  |  0  |  1  |

# Subnet

Used to break down larger networks into smaller ones (subnets).

IP addresses have 2 parts, and the subnet mask determines which part is the network and the host, by masking the bits used for the network.

```bash
# Network address
192.168.100.0

# Host addresses
192.168.100.1
192.168.100.2
192.168.100.3
```

```
IP address        192.168.1.0        11000000.10101000.00000001.00000000

Subnet Mask       255.255.255.0      11111111.11111111.11111111.00000000
                                     -------------------------- --------
                                                Network           Host

Subnet Mask       255.255.224.0      11111111.11111111.111 00000.00000000
                                     --------------------- --------------
                                              Network           Host
```

```bash
# CIDR Notation - Classless Inter-Domain Routing (slash notation)

192.168.1.0 /24   =   24 bit subnet mask

11111111.11111111.11111111.00000000   =   255.255.255.0
```

| CIDR |   Subnet mask   |               Binary                | Networks | Hosts |
| :--: | :-------------: | :---------------------------------: | :------: | :---: |
| /24  |  255.255.255.0  | 11111111.11111111.11111111.00000000 |    1     |  254  |
| /25  | 255.255.255.128 | 11111111.11111111.11111111.10000000 |    2     |  126  |
| /26  | 255.255.255.192 | 11111111.11111111.11111111.11000000 |    4     |  62   |
| /27  | 255.255.255.224 | 11111111.11111111.11111111.11100000 |    8     |  30   |
| /28  | 255.255.255.240 | 11111111.11111111.11111111.11110000 |    16    |  14   |
| /29  | 255.255.255.248 | 11111111.11111111.11111111.11111000 |    32    |   6   |
| /30  | 255.255.255.252 | 11111111.11111111.11111111.11111100 |    64    |   2   |
| /31  | 255.255.255.254 | 11111111.11111111.11111111.11111110 |   128    |   0   |

Classes

|           User            | Class | First octet address | Default subnet mask | Hosts |
| :-----------------------: | :---: | :-----------------: | :-----------------: | :---: |
|            ISP            |   A   |       1 - 126       |      255.0.0.0      | 16 M  |
|    Large organization     |   A   |      128 - 191      |     255.255.0.0     | 65 K  |
| Small organization / home |   A   |      192 - 223      |    255.255.255.0    |  254  |

# DHCP - Dynamic Host Configuration Protocol

A service that runs on a router or server, which automatically assigns:

-   IP address
-   Subnet mask
-   Default gateway
-   DNS server

It **leases** the IP addresses to all devices connected to it. It leases them instead of giving to prevent running out of them ex. when a PC is removed, it would take the address with it.

The computers given the addresses ping the DHCP server on intervals to renew their lease, otherwise the address returns to the pool.

You can assign a static address via reservation. They are usually assigned to printers, routers, servers...

Before this service, you had to manually input all the parameters for each device in the network.

# NAT - Network Address Translation

Private to public IP address conversion, and vice-versa.

# Default Gateway (Router)

Majority of times it's a router.

Forwards data from one network to another.

**Gateway** is the router address we are talking to in order to connect to the rest of the network/internet.
