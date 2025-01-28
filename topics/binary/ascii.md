# ASCII vs UTF-8

### **ASCII**:

-   **Range**: ASCII uses 7 bits per character, which gives it a character set of 128 symbols (0–127). These include English letters (both uppercase and lowercase), digits, punctuation marks, and control characters.
-   **Usage**: ASCII is primarily used for encoding English text and simple characters.
-   **Limitations**: It only supports a limited set of characters, mainly for English. Non-English characters and symbols, such as accented letters or characters from other languages, cannot be represented in ASCII.

### **UTF-8**:

-   **Range**: UTF-8 is a variable-width character encoding. It can represent every character in the Unicode standard (which includes over 143,000 characters from various languages, symbols, and emojis) and uses 1 to 4 bytes per character.
    -   For characters in the ASCII range (0–127), UTF-8 uses the same 1 byte as ASCII.
    -   For characters beyond the ASCII range, UTF-8 uses 2, 3, or 4 bytes.
-   **Usage**: UTF-8 is widely used on the web and in modern applications because it supports a much broader range of characters and is backward compatible with ASCII.
-   **Advantages**:
    -   It supports all characters and symbols, including non-English characters, emojis, and various scripts.
    -   It is more flexible and future-proof because it can represent any Unicode character.
    -   It's backward compatible with ASCII, so any ASCII text is also valid UTF-8 text.

### **Summary**:

-   **ASCII** is simpler and more limited, designed for English and basic control characters.
-   **UTF-8** is more versatile and can handle virtually all characters from different languages, including symbols and emojis, while being compatible with ASCII for the first 128 characters.

# ASCII Characters

| Decimal | Binary   | Hex | Hex | Hex  | ASCII       | Name                      | Description                                                                         |
| :------ | :------- | :-- | :-- | :--- | :---------- | :------------------------ | :---------------------------------------------------------------------------------- |
| 0       | 00000000 | 00  | 00h | 0x00 | NUL         | Null                      | Used as sign of end of a string or to fill an unallocated data space                |
| 1       | 00000001 | 01  | 01h | 0x01 | SOH         | Start of Heading          | Defines the beginning of the message header block                                   |
| 2       | 00000010 | 02  | 02h | 0x02 | STX         | Start of Text             | Specifies the beginning of the text block and terminates the header block           |
| 3       | 00000011 | 03  | 03h | 0x03 | ETX         | End of Text               | Often used as a “break” character (Ctrl C) to terminate a program                   |
| 4       | 00000100 | 04  | 04h | 0x04 | EOT         | End of Transmission       | Control character that indicates end-of-file on a terminal                          |
| 5       | 00000101 | 05  | 05h | 0x05 | ENQ         | Enquiry                   | Transmission-control character that requests a response from the receiving end      |
| 6       | 00000110 | 06  | 06h | 0x06 | ACK         | Acknowledgement           | Response to an ENQ, or an indication of successful receipt of a message             |
| 7       | 00000111 | 07  | 07h | 0x07 | BEL         | Bell                      | Device control code informs the system that it should beep                          |
| 8       | 00001000 | 08  | 08h | 0x08 | BS          | Backspace                 | Moves the cursor one position leftwards and removes its character                   |
| 9       | 00001001 | 09  | 09h | 0x09 | HT          | Horizontal Tab            | Moves the cursor to the next tab stop (e.g., by pressing the Tab key)               |
| 10      | 00001010 | 0A  | 0Ah | 0x0A | LF          | Line Feed                 | It is used to indicate the end of a line (e.g., by pressing the Enter key on UNIX)  |
| 11      | 00001011 | 0B  | 0Bh | 0x0B | VT          | Vertical Tab              | Place the form at the next line tab stop                                            |
| 12      | 00001100 | 0C  | 0Ch | 0x0C | FF          | Form Feed                 | Page-breaking character indicates that the following content is part of a new page  |
| 13      | 00001101 | 0D  | 0Dh | 0x0D | CR          | Carriage Return           | It is used to indicate the end of a line (e.g., by pressing the Enter key on MacOS) |
| 14      | 00001110 | 0E  | 0Eh | 0x0E | SO          | Shift Out                 | Known as Control-N, it switches to an alternate character set                       |
| 15      | 00001111 | 0F  | 0Fh | 0x0F | SI          | Shift In                  | Known as Control-O, it returns the regular character set after Shift Out            |
| 16      | 00010000 | 10  | 10h | 0x10 | DLE         | Data Link Escape          | Force the following octets to be interpreted as raw data                            |
| 17      | 00010001 | 11  | 11h | 0x11 | DC1         | Device Control 1          | Known as XON, it resumes or turns on the device                                     |
| 18      | 00010010 | 12  | 12h | 0x12 | DC2         | Device Control 2          | The same as DC1                                                                     |
| 19      | 00010011 | 13  | 13h | 0x13 | DC3         | Device Control 3          | Known as XOFF, it pauses or turns off the device                                    |
| 20      | 00010100 | 14  | 14h | 0x14 | DC4         | Device Control 4          | The same as DC3                                                                     |
| 21      | 00010101 | 15  | 15h | 0x15 | NAK         | Negative Acknowledgement  | Reply to the ENQ message informing the sender of an occurred error                  |
| 22      | 00010110 | 16  | 16h | 0x16 | SYN         | Synchronous Idle          | Used in hardware synchronization procedures                                         |
| 23      | 00010111 | 17  | 17h | 0x17 | ETB         | End of Transmission Block | It is used when transferring data by blocks and serves as a separator               |
| 24      | 00011000 | 18  | 18h | 0x18 | CAN         | Cancel                    | Tells the device to ignore the data that was sent before this character             |
| 25      | 00011001 | 19  | 19h | 0x19 | EM          | End of Medium             | Used to notify that the paper or magnetic tape reached the end                      |
| 26      | 00011010 | 1A  | 1Ah | 0x1A | SUB         | Substitute                | May be used to replace a character or to undo the last action                       |
| 27      | 00011011 | 1B  | 1Bh | 0x1B | ESC         | Escape                    | Usually it is associated with the Esc key (for example, to close a popup)           |
| 28      | 00011100 | 1C  | 1Ch | 0x1C | FS          | File Separator            | It is used as a sign of dividing the data stream into files                         |
| 29      | 00011101 | 1D  | 1Dh | 0x1D | GS          | Group Separator           | It is used as a sign of dividing the data stream into groups                        |
| 30      | 00011110 | 1E  | 1Eh | 0x1E | RS          | Record Separator          | It is a record separator in the data stream                                         |
| 31      | 00011111 | 1F  | 1Fh | 0x1F | US          | Unit Separator            | It is intended for separating elements in the data stream                           |
| 32      | 00100000 | 20  | 20h | 0x20 |             | Space                     | Blank area that separates characters (can be written by pressing the Space key)     |
| 33      | 00100001 | 21  | 21h | 0x21 | !           | Exclamation mark          | Shift 1                                                                             |
| 34      | 00100010 | 22  | 22h | 0x22 | "           | Quotation mark            | Shift '                                                                             |
| 35      | 00100011 | 23  | 23h | 0x23 | #           | Number sign               | Shift 3                                                                             |
| 36      | 00100100 | 24  | 24h | 0x24 | $           | Dollar sign               | Shift 4                                                                             |
| 37      | 00100101 | 25  | 25h | 0x25 | %           | Percent sign              | Shift 5                                                                             |
| 38      | 00100110 | 26  | 26h | 0x26 | &           | Ampersand                 | Shift 7                                                                             |
| 39      | 00100111 | 27  | 27h | 0x27 | '           | Apostrophe                |                                                                                     |
| 40      | 00101000 | 28  | 28h | 0x28 | (           | Left parenthesis          | Shift 9                                                                             |
| 41      | 00101001 | 29  | 29h | 0x29 | )           | Right parenthesis         | Shift 10                                                                            |
| 42      | 00101010 | 2A  | 2Ah | 0x2A | \*          | Asterisk                  | Shift 8                                                                             |
| 43      | 00101011 | 2B  | 2Bh | 0x2B | +           | Plus sign                 | Shift =                                                                             |
| 44      | 00101100 | 2C  | 2Ch | 0x2C | ,           | Comma                     |                                                                                     |
| 45      | 00101101 | 2D  | 2Dh | 0x2D | -           | Hyphen-minus              |                                                                                     |
| 46      | 00101110 | 2E  | 2Eh | 0x2E | .           | Full stop (dot)           |                                                                                     |
| 47      | 00101111 | 2F  | 2Fh | 0x2F | /           | Slash                     |                                                                                     |
| 48      | 00110000 | 30  | 30h | 0x30 | 0           | Zero                      |                                                                                     |
| 49      | 00110001 | 31  | 31h | 0x31 | 1           | One                       |                                                                                     |
| 50      | 00110010 | 32  | 32h | 0x32 | 2           | Two                       |                                                                                     |
| 51      | 00110011 | 33  | 33h | 0x33 | 3           | Three                     |                                                                                     |
| 52      | 00110100 | 34  | 34h | 0x34 | 4           | Four                      |                                                                                     |
| 53      | 00110101 | 35  | 35h | 0x35 | 5           | Five                      |                                                                                     |
| 54      | 00110110 | 36  | 36h | 0x36 | 6           | Six                       |                                                                                     |
| 55      | 00110111 | 37  | 37h | 0x37 | 7           | Seven                     |                                                                                     |
| 56      | 00111000 | 38  | 38h | 0x38 | 8           | Eight                     |                                                                                     |
| 57      | 00111001 | 39  | 39h | 0x39 | 9           | Nine                      |                                                                                     |
| 58      | 00111010 | 3A  | 3Ah | 0x3A | :           | Colon                     | Shift ;                                                                             |
| 59      | 00111011 | 3B  | 3Bh | 0x3B | ;           | Semicolon                 |                                                                                     |
| 60      | 00111100 | 3C  | 3Ch | 0x3C | <           | Less-than sign            | Shift ,                                                                             |
| 61      | 00111101 | 3D  | 3Dh | 0x3D | =           | Equals sign               |                                                                                     |
| 62      | 00111110 | 3E  | 3Eh | 0x3E | >           | Greater-than sign         | Shift .                                                                             |
| 63      | 00111111 | 3F  | 3Fh | 0x3F | ?           | Question mark             | Shift /                                                                             |
| 64      | 01000000 | 40  | 40h | 0x40 | @           | At sign                   | Shift 2                                                                             |
| 65      | 01000001 | 41  | 41h | 0x41 | A           | Uppercase “A”             |                                                                                     |
| 66      | 01000010 | 42  | 42h | 0x42 | B           | Uppercase “B”             |                                                                                     |
| 67      | 01000011 | 43  | 43h | 0x43 | C           | Uppercase “C”             |                                                                                     |
| 68      | 01000100 | 44  | 44h | 0x44 | D           | Uppercase “D”             |                                                                                     |
| 69      | 01000101 | 45  | 45h | 0x45 | E           | Uppercase “E”             |                                                                                     |
| 70      | 01000110 | 46  | 46h | 0x46 | F           | Uppercase “F”             |                                                                                     |
| 71      | 01000111 | 47  | 47h | 0x47 | G           | Uppercase “G”             |                                                                                     |
| 72      | 01001000 | 48  | 48h | 0x48 | H           | Uppercase “H”             |                                                                                     |
| 73      | 01001001 | 49  | 49h | 0x49 | I           | Uppercase “I”             |                                                                                     |
| 74      | 01001010 | 4A  | 4Ah | 0x4A | J           | Uppercase “J”             |                                                                                     |
| 75      | 01001011 | 4B  | 4Bh | 0x4B | K           | Uppercase “K”             |                                                                                     |
| 76      | 01001100 | 4C  | 4Ch | 0x4C | L           | Uppercase “L”             |                                                                                     |
| 77      | 01001101 | 4D  | 4Dh | 0x4D | M           | Uppercase “M”             |                                                                                     |
| 78      | 01001110 | 4E  | 4Eh | 0x4E | N           | Uppercase “N”             |                                                                                     |
| 79      | 01001111 | 4F  | 4Fh | 0x4F | O           | Uppercase “O”             |                                                                                     |
| 80      | 01010000 | 50  | 50h | 0x50 | P           | Uppercase “P”             |                                                                                     |
| 81      | 01010001 | 51  | 51h | 0x51 | Q           | Uppercase “Q”             |                                                                                     |
| 82      | 01010010 | 52  | 52h | 0x52 | R           | Uppercase “R”             |                                                                                     |
| 83      | 01010011 | 53  | 53h | 0x53 | S           | Uppercase “S”             |                                                                                     |
| 84      | 01010100 | 54  | 54h | 0x54 | T           | Uppercase “T”             |                                                                                     |
| 85      | 01010101 | 55  | 55h | 0x55 | U           | Uppercase “U”             |                                                                                     |
| 86      | 01010110 | 56  | 56h | 0x56 | V           | Uppercase “V”             |                                                                                     |
| 87      | 01010111 | 57  | 57h | 0x57 | W           | Uppercase “W”             |                                                                                     |
| 88      | 01011000 | 58  | 58h | 0x58 | X           | Uppercase “X”             |                                                                                     |
| 89      | 01011001 | 59  | 59h | 0x59 | Y           | Uppercase “Y”             |                                                                                     |
| 90      | 01011010 | 5A  | 5Ah | 0x5A | Z           | Uppercase “Z”             |                                                                                     |
| 91      | 01011011 | 5B  | 5Bh | 0x5B | [           | Left square bracket       |                                                                                     |
| 92      | 01011100 | 5C  | 5Ch | 0x5C | \|Backslash |                           |
| 93      | 01011101 | 5D  | 5Dh | 0x5D | ]           | Right square bracket      |                                                                                     |
| 94      | 01011110 | 5E  | 5Eh | 0x5E | ^           | Caret                     | Shift 6                                                                             |
| 95      | 01011111 | 5F  | 5Fh | 0x5F | \_          | Underscore                | Shift -                                                                             |
| 96      | 01100000 | 60  | 60h | 0x60 | `           | Grave accent              |                                                                                     |
| 97      | 01100001 | 61  | 61h | 0x61 | a           | Lowercase “a”             |                                                                                     |
| 98      | 01100010 | 62  | 62h | 0x62 | b           | Lowercase “b”             |                                                                                     |
| 99      | 01100011 | 63  | 63h | 0x63 | c           | Lowercase “c”             |                                                                                     |
| 100     | 01100100 | 64  | 64h | 0x64 | d           | Lowercase “d”             |                                                                                     |
| 101     | 01100101 | 65  | 65h | 0x65 | e           | Lowercase “e”             |                                                                                     |
| 102     | 01100110 | 66  | 66h | 0x66 | f           | Lowercase “f”             |                                                                                     |
| 103     | 01100111 | 67  | 67h | 0x67 | g           | Lowercase “g”             |                                                                                     |
| 104     | 01101000 | 68  | 68h | 0x68 | h           | Lowercase “h”             |                                                                                     |
| 105     | 01101001 | 69  | 69h | 0x69 | i           | Lowercase “i”             |                                                                                     |
| 106     | 01101010 | 6A  | 6Ah | 0x6A | j           | Lowercase “j”             |                                                                                     |
| 107     | 01101011 | 6B  | 6Bh | 0x6B | k           | Lowercase “k”             |                                                                                     |
| 108     | 01101100 | 6C  | 6Ch | 0x6C | l           | Lowercase “l”             |                                                                                     |
| 109     | 01101101 | 6D  | 6Dh | 0x6D | m           | Lowercase “m”             |                                                                                     |
| 110     | 01101110 | 6E  | 6Eh | 0x6E | n           | Lowercase “n”             |                                                                                     |
| 111     | 01101111 | 6F  | 6Fh | 0x6F | o           | Lowercase “o”             |                                                                                     |
| 112     | 01110000 | 70  | 70h | 0x70 | p           | Lowercase “p”             |                                                                                     |
| 113     | 01110001 | 71  | 71h | 0x71 | q           | Lowercase “q”             |                                                                                     |
| 114     | 01110010 | 72  | 72h | 0x72 | r           | Lowercase “r”             |                                                                                     |
| 115     | 01110011 | 73  | 73h | 0x73 | s           | Lowercase “s”             |                                                                                     |
| 116     | 01110100 | 74  | 74h | 0x74 | t           | Lowercase “t”             |                                                                                     |
| 117     | 01110101 | 75  | 75h | 0x75 | u           | Lowercase “u”             |                                                                                     |
| 118     | 01110110 | 76  | 76h | 0x76 | v           | Lowercase “v”             |                                                                                     |
| 119     | 01110111 | 77  | 77h | 0x77 | w           | Lowercase “w”             |                                                                                     |
| 120     | 01111000 | 78  | 78h | 0x78 | x           | Lowercase “x”             |                                                                                     |
| 121     | 01111001 | 79  | 79h | 0x79 | y           | Lowercase “y”             |                                                                                     |
| 122     | 01111010 | 7A  | 7Ah | 0x7A | z           | Lowercase “z”             |                                                                                     |
| 123     | 01111011 | 7B  | 7Bh | 0x7B | {           | Left curly bracket        | Shift [                                                                             |
| 124     | 01111100 | 7C  | 7Ch | 0x7C | \|          | Vertical bar              | Shift \|                                                                            |
| 125     | 01111101 | 7D  | 7Dh | 0x7D | }           | Right curly bracket       | Shift ]                                                                             |
| 126     | 01111110 | 7E  | 7Eh | 0x7E | ~           | Tilde                     | Shift `                                                                             |
| 127     | 01111111 | 7F  | 7Fh | 0x7F | DEL         | Delete                    | Marks deleted symbols on paper tape                                                 |
| 128     | 10000000 | 80  | 80h | 0x80 | €           |                           |                                                                                     |
| 129     | 10000001 | 81  | 81h | 0x81 |             |                           |                                                                                     |
| 130     | 10000010 | 82  | 82h | 0x82 | ‚           |                           |                                                                                     |
| 131     | 10000011 | 83  | 83h | 0x83 | ƒ           |                           |                                                                                     |
| 132     | 10000100 | 84  | 84h | 0x84 | „           |                           |                                                                                     |
| 133     | 10000101 | 85  | 85h | 0x85 | …           |                           |                                                                                     |
| 134     | 10000110 | 86  | 86h | 0x86 | †           |                           |                                                                                     |
| 135     | 10000111 | 87  | 87h | 0x87 | ‡           |                           |                                                                                     |
| 136     | 10001000 | 88  | 88h | 0x88 | ˆ           |                           |                                                                                     |
| 137     | 10001001 | 89  | 89h | 0x89 | ‰           |                           |                                                                                     |
| 138     | 10001010 | 8A  | 8Ah | 0x8A | Š           |                           |                                                                                     |
| 139     | 10001011 | 8B  | 8Bh | 0x8B | ‹           |                           |                                                                                     |
| 140     | 10001100 | 8C  | 8Ch | 0x8C | Œ           |                           |                                                                                     |
| 141     | 10001101 | 8D  | 8Dh | 0x8D |             |                           |                                                                                     |
| 142     | 10001110 | 8E  | 8Eh | 0x8E | Ž           |                           |                                                                                     |
| 143     | 10001111 | 8F  | 8Fh | 0x8F |             |                           |                                                                                     |
| 144     | 10010000 | 90  | 90h | 0x90 |             |                           |                                                                                     |
| 145     | 10010001 | 91  | 91h | 0x91 | ‘           |                           |                                                                                     |
| 146     | 10010010 | 92  | 92h | 0x92 | ’           |                           |                                                                                     |
| 147     | 10010011 | 93  | 93h | 0x93 | “           |                           |                                                                                     |
| 148     | 10010100 | 94  | 94h | 0x94 | ”           |                           |                                                                                     |
| 149     | 10010101 | 95  | 95h | 0x95 | •           |                           |                                                                                     |
| 150     | 10010110 | 96  | 96h | 0x96 | –           |                           |                                                                                     |
| 151     | 10010111 | 97  | 97h | 0x97 | —           |                           |                                                                                     |
| 152     | 10011000 | 98  | 98h | 0x98 | ˜           |                           |                                                                                     |
| 153     | 10011001 | 99  | 99h | 0x99 | ™           |                           |                                                                                     |
| 154     | 10011010 | 9A  | 9Ah | 0x9A | š           |                           |                                                                                     |
| 155     | 10011011 | 9B  | 9Bh | 0x9B | ›           |                           |                                                                                     |
| 156     | 10011100 | 9C  | 9Ch | 0x9C | œ           |                           |                                                                                     |
| 157     | 10011101 | 9D  | 9Dh | 0x9D |             |                           |                                                                                     |
| 158     | 10011110 | 9E  | 9Eh | 0x9E | ž           |                           |                                                                                     |
| 159     | 10011111 | 9F  | 9Fh | 0x9F | Ÿ           |                           |                                                                                     |
| 160     | 10100000 | A0  | A0h | 0xA0 |             |                           |                                                                                     |
| 161     | 10100001 | A1  | A1h | 0xA1 | ¡           |                           |                                                                                     |
| 162     | 10100010 | A2  | A2h | 0xA2 | ¢           |                           |                                                                                     |
| 163     | 10100011 | A3  | A3h | 0xA3 | £           |                           |                                                                                     |
| 164     | 10100100 | A4  | A4h | 0xA4 | ¤           |                           |                                                                                     |
| 165     | 10100101 | A5  | A5h | 0xA5 | ¥           |                           |                                                                                     |
| 166     | 10100110 | A6  | A6h | 0xA6 | ¦           |                           |                                                                                     |
| 167     | 10100111 | A7  | A7h | 0xA7 | §           |                           |                                                                                     |
| 168     | 10101000 | A8  | A8h | 0xA8 | ¨           |                           |                                                                                     |
| 169     | 10101001 | A9  | A9h | 0xA9 | ©           |                           |                                                                                     |
| 170     | 10101010 | AA  | AAh | 0xAA | ª           |                           |                                                                                     |
| 171     | 10101011 | AB  | ABh | 0xAB | «           |                           |                                                                                     |
| 172     | 10101100 | AC  | ACh | 0xAC | ¬           |                           |                                                                                     |
| 173     | 10101101 | AD  | ADh | 0xAD | ­           |                           |                                                                                     |
| 174     | 10101110 | AE  | AEh | 0xAE | ®           |                           |                                                                                     |
| 175     | 10101111 | AF  | AFh | 0xAF | ¯           |                           |                                                                                     |
| 176     | 10110000 | B0  | B0h | 0xB0 | °           |                           |                                                                                     |
| 177     | 10110001 | B1  | B1h | 0xB1 | ±           |                           |                                                                                     |
| 178     | 10110010 | B2  | B2h | 0xB2 | ²           |                           |                                                                                     |
| 179     | 10110011 | B3  | B3h | 0xB3 | ³           |                           |                                                                                     |
| 180     | 10110100 | B4  | B4h | 0xB4 | ´           |                           |                                                                                     |
| 181     | 10110101 | B5  | B5h | 0xB5 | µ           |                           |                                                                                     |
| 182     | 10110110 | B6  | B6h | 0xB6 | ¶           |                           |                                                                                     |
| 183     | 10110111 | B7  | B7h | 0xB7 | ·           |                           |                                                                                     |
| 184     | 10111000 | B8  | B8h | 0xB8 | ¸           |                           |                                                                                     |
| 185     | 10111001 | B9  | B9h | 0xB9 | ¹           |                           |                                                                                     |
| 186     | 10111010 | BA  | BAh | 0xBA | º           |                           |                                                                                     |
| 187     | 10111011 | BB  | BBh | 0xBB | »           |                           |                                                                                     |
| 188     | 10111100 | BC  | BCh | 0xBC | ¼           |                           |                                                                                     |
| 189     | 10111101 | BD  | BDh | 0xBD | ½           |                           |                                                                                     |
| 190     | 10111110 | BE  | BEh | 0xBE | ¾           |                           |                                                                                     |
| 191     | 10111111 | BF  | BFh | 0xBF | ¿           |                           |                                                                                     |
| 192     | 11000000 | C0  | C0h | 0xC0 | À           |                           |                                                                                     |
| 193     | 11000001 | C1  | C1h | 0xC1 | Á           |                           |                                                                                     |
| 194     | 11000010 | C2  | C2h | 0xC2 | Â           |                           |                                                                                     |
| 195     | 11000011 | C3  | C3h | 0xC3 | Ã           |                           |                                                                                     |
| 196     | 11000100 | C4  | C4h | 0xC4 | Ä           |                           |                                                                                     |
| 197     | 11000101 | C5  | C5h | 0xC5 | Å           |                           |                                                                                     |
| 198     | 11000110 | C6  | C6h | 0xC6 | Æ           |                           |                                                                                     |
| 199     | 11000111 | C7  | C7h | 0xC7 | Ç           |                           |                                                                                     |
| 200     | 11001000 | C8  | C8h | 0xC8 | È           |                           |                                                                                     |
| 201     | 11001001 | C9  | C9h | 0xC9 | É           |                           |                                                                                     |
| 202     | 11001010 | CA  | CAh | 0xCA | Ê           |                           |                                                                                     |
| 203     | 11001011 | CB  | CBh | 0xCB | Ë           |                           |                                                                                     |
| 204     | 11001100 | CC  | CCh | 0xCC | Ì           |                           |                                                                                     |
| 205     | 11001101 | CD  | CDh | 0xCD | Í           |                           |                                                                                     |
| 206     | 11001110 | CE  | CEh | 0xCE | Î           |                           |                                                                                     |
| 207     | 11001111 | CF  | CFh | 0xCF | Ï           |                           |                                                                                     |
| 208     | 11010000 | D0  | D0h | 0xD0 | Ð           |                           |                                                                                     |
| 209     | 11010001 | D1  | D1h | 0xD1 | Ñ           |                           |                                                                                     |
| 210     | 11010010 | D2  | D2h | 0xD2 | Ò           |                           |                                                                                     |
| 211     | 11010011 | D3  | D3h | 0xD3 | Ó           |                           |                                                                                     |
| 212     | 11010100 | D4  | D4h | 0xD4 | Ô           |                           |                                                                                     |
| 213     | 11010101 | D5  | D5h | 0xD5 | Õ           |                           |                                                                                     |
| 214     | 11010110 | D6  | D6h | 0xD6 | Ö           |                           |                                                                                     |
| 215     | 11010111 | D7  | D7h | 0xD7 | ×           |                           |                                                                                     |
| 216     | 11011000 | D8  | D8h | 0xD8 | Ø           |                           |                                                                                     |
| 217     | 11011001 | D9  | D9h | 0xD9 | Ù           |                           |                                                                                     |
| 218     | 11011010 | DA  | DAh | 0xDA | Ú           |                           |                                                                                     |
| 219     | 11011011 | DB  | DBh | 0xDB | Û           |                           |                                                                                     |
| 220     | 11011100 | DC  | DCh | 0xDC | Ü           |                           |                                                                                     |
| 221     | 11011101 | DD  | DDh | 0xDD | Ý           |                           |                                                                                     |
| 222     | 11011110 | DE  | DEh | 0xDE | Þ           |                           |                                                                                     |
| 223     | 11011111 | DF  | DFh | 0xDF | ß           |                           |                                                                                     |
| 224     | 11100000 | E0  | E0h | 0xE0 | à           |                           |                                                                                     |
| 225     | 11100001 | E1  | E1h | 0xE1 | á           |                           |                                                                                     |
| 226     | 11100010 | E2  | E2h | 0xE2 | â           |                           |                                                                                     |
| 227     | 11100011 | E3  | E3h | 0xE3 | ã           |                           |                                                                                     |
| 228     | 11100100 | E4  | E4h | 0xE4 | ä           |                           |                                                                                     |
| 229     | 11100101 | E5  | E5h | 0xE5 | å           |                           |                                                                                     |
| 230     | 11100110 | E6  | E6h | 0xE6 | æ           |                           |                                                                                     |
| 231     | 11100111 | E7  | E7h | 0xE7 | ç           |                           |                                                                                     |
| 232     | 11101000 | E8  | E8h | 0xE8 | è           |                           |                                                                                     |
| 233     | 11101001 | E9  | E9h | 0xE9 | é           |                           |                                                                                     |
| 234     | 11101010 | EA  | EAh | 0xEA | ê           |                           |                                                                                     |
| 235     | 11101011 | EB  | EBh | 0xEB | ë           |                           |                                                                                     |
| 236     | 11101100 | EC  | ECh | 0xEC | ì           |                           |                                                                                     |
| 237     | 11101101 | ED  | EDh | 0xED | í           |                           |                                                                                     |
| 238     | 11101110 | EE  | EEh | 0xEE | î           |                           |                                                                                     |
| 239     | 11101111 | EF  | EFh | 0xEF | ï           |                           |                                                                                     |
| 240     | 11110000 | F0  | F0h | 0xF0 | ð           |                           |                                                                                     |
| 241     | 11110001 | F1  | F1h | 0xF1 | ñ           |                           |                                                                                     |
| 242     | 11110010 | F2  | F2h | 0xF2 | ò           |                           |                                                                                     |
| 243     | 11110011 | F3  | F3h | 0xF3 | ó           |                           |                                                                                     |
| 244     | 11110100 | F4  | F4h | 0xF4 | ô           |                           |                                                                                     |
| 245     | 11110101 | F5  | F5h | 0xF5 | õ           |                           |                                                                                     |
| 246     | 11110110 | F6  | F6h | 0xF6 | ö           |                           |                                                                                     |
| 247     | 11110111 | F7  | F7h | 0xF7 | ÷           |                           |                                                                                     |
| 248     | 11111000 | F8  | F8h | 0xF8 | ø           |                           |                                                                                     |
| 249     | 11111001 | F9  | F9h | 0xF9 | ù           |                           |                                                                                     |
| 250     | 11111010 | FA  | FAh | 0xFA | ú           |                           |                                                                                     |
| 251     | 11111011 | FB  | FBh | 0xFB | û           |                           |                                                                                     |
| 252     | 11111100 | FC  | FCh | 0xFC | ü           |                           |                                                                                     |
| 253     | 11111101 | FD  | FDh | 0xFD | ý           |                           |                                                                                     |
| 254     | 11111110 | FE  | FEh | 0xFE | þ           |                           |                                                                                     |
| 255     | 11111111 | FF  | FFh | 0xFF | ÿ           |                           |                                                                                     |
