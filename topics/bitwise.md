
# Bitwise OR

Sets each bit to 1 if one of two bits is 1.

```javascript
// 1 = 00000001
// 2 = 00000010

console.log(1 | 2)
// 3 = 00000011
```

# Bitwise AND

Sets each bit to 1 if both bits are 1.

```javascript
// 1 = 00000001
// 2 = 00000010

console.log(1 & 2)
// 0 = 00000000
```

# Bitwise XOR

Sets each bit to 1 if only one of two bits is 1.

```javascript
// 10 = 00001010

console.log(1 ^ 2)
// 6 = 000000110
```

# Bitwise NOT

Inverts all the bits.

# Shift Left

Add n zeros to the right i.e. remove n bits from the left. (binary to number)

```
00000 111 (7) << 1 = 0000 111 0 (14)
00000 101 (5) << 3 = 00 101 000 (40)
```

# Shift Right

Add n zeros to the left i.e. remove n bits from the right. (binary to number)

```
00 101010 (42) >> 4 = 000000 10 (2)
00000 111  (7) >> 1 = 000000 11 (3)
00 110011 (51) >> 3 = 00000 110 (6)
```
