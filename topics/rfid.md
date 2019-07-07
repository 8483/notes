# Standards

| Family | Frequency | Protocol| Storage | Writable | Reader
|---|---|---|---|---|---|
Low Frequency (LF) | 125/134 kHz | EM4100 | UID 4 bytes | No | RDM-6300 (Coil antenna)
High Frequency (HF) | 13.56 MHz | Mifare Classic | UID 4 bytes | 1 KB | MFRC-522 (Loop antenna)
Ultra High Frequency (UHF) | 868 MHz | 1386.00 |

The UIDs are hard-coded by the manufacturer and cannot be changed.

All cards contain a chip and an antenna. They are passive i.e. get the energy from the reader.

# Wiegand

This is a transmission protocol which connects the RFID reader with a controller.

The Wiegand interface has two data lines, DATA0 and DATA1.  These lines are normall held high at 5V.
- When a 0 is sent, DATA0 drops to 0V for a few µs.  
- When a 1 is sent, DATA1 drops to 0V for a few µs. 

There are a few ms between the pulses.

It transmits the UID in 2 formats:  

- W26 - Transports only first 3 bytes of the UID.
- W34 - Transports the whole UID (4 bytes). Connect brown wire to ground to get this format.

If the correct UIDs are not transported via serial, the D0 and D1 may need to be swapped.

![Sketch](../pics/rfid_wiegand_arduino.jpg)