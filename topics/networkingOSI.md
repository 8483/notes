# OSI Model

OSI/IP : Used to identify the devices corresponding to the protocols , and their relationships, that are involved during this communication

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

For simplicity and more realistic use, it bundles the OSI layers above it:

-   **Session layer** - open/close transport layer connections.
-   **Presentation layer** - Lets programs exchange data.
-   **Application layer** - Actual protocol spoken over these connections.

The `TCP/IP` model calls everything above the transport layer the application layer. It includes protocols like HTTP, SMTP, LDAP...
