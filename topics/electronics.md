# Terminology

VIN - Voltage that is currently supplied to the board.  
VCC - Input pin on the board if you want to power it from a pin instead of a port.

## Bus

A set of wires which transfer data. One wire can carry one bit: a 0 (low voltage) or 1 (high voltage). A bus is just a collection of wires, so a 4-bit bus has four wires and can carry four bits.

## Baud

Measure for communication speed over a data channel.

Equivalent to bits per second.

## I2C - Inter-Integrated Circuit

It's a bus that allows easy communication between components which reside on the same circuit board.

SDA - Serial data  
SCL - Serial clock

## SPI - Serial Peripheral Interface

MISO - Master In Slave Out  
MOSI - Master Out Slave In

## UART - Universal asynchronous receiver-transmitter

TxD - Transmitter, carries data from DTE to DCE.  
RxD - Receiver, carries data from DCE to DTE.

## RS-232

Standard for serial communication transmission of data.

It formally defines the signals connecting:

-   DTE (data terminal equipment) ex. computer terminal.
-   DCE (data communication equipment) ex. modem.
