# MAC

Physical address of a device.

IP address locates the device, MAC identifies it.

Devices need it to communicate on the local network.

They get it with ARP.

# ARP - Address Resolution Protocol

Used to resolve IP addresses to MAC addresses.

The broadcast does not go beyond the router.

```
Device A - Who is 10.0.0.4? I need the MAC address.

Device B - I am 10.0.0.4. I will send you the MAC address.
```

This is all cached to avoid sending the signal again.

```
arp -a
```

IP addresses can be matched to MAC manually.

```
arp -s 10.0.0.4 90-02-7b-c2-c0-67
```
