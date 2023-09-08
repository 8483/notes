# Overview

Hubs and switches create networks.  
Routers connect networks.

```
PC 1 -----|                            |----- Router
          |                            |
PC 2 -----|----- Router --- Modem -----|----- Router
          |                            |
Phone ----|                            |----- Router

----------------------------------     -------------
        Local network                     Internet
```

# Connect Networks

## **Modem**

Converts the internet's analog signals to digital ones for your computer, and vice-versa.

Establishes a connection to your ISP, and bings the internet into your home or business.

Comes before the router, usually bundled together.

You can connect directly to the modem, but a router is needed for multiple devices.

Short for modulator/demodulator.

## **Router**

The **gateway** of a network.

Routes data from one network to another based on their IP address.

It determines if the data is for its own network and receives it, or forwards it to another one if it's not.

Comes after the modem, usually bundled together.

Passes your internet connection to all your devices via its built in switch for wired connections or access point for wireless ones.

# Create and expand networks

These devices do not read IP addresses, only MAC.

## **Switch**

Detects and identifies **specific** devices connected to it.

Routes a signal to a specific device connected via ethernet, by storing its MAC address.

Routers have one built in.

It's a smart hub.

## **Hub**

Only detects devices connected to it.

Routes a signal to **all** connected devices via ethernet.

# Drivers

Drivers act like a translator between the device they control, and the other programs in your system.

For instance, lets say you want to print "hello world" on a piece of paper. So you type that in to notepad, and click print.
Windows passes the printer driver the document and says "Print this."
The printer driver takes the information it gets, then turns to the printer and says "I want you to print out the following letters 'h' then 'e' then 'l' and so on.

Except it's all a lot more complicated than that because we don't send individual letters to a printer any more, we send a lot more detailed information about where to put ink or toner down, how to mix and layer ink or toner to get finely detailed pictures to come out clearly, and a lot of printers actually are smart enough to do these things on their own under certain conditions (like printing out pictures stored on a USB drive plugged directly to a printer)
