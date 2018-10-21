base = how bytes are represented

# base-2 - Binary

Most basic representation. 1 byte is 8 binary characters or bits.

| 128           | 64            | 32            | 16            | 8             | 4             | 2             | 1             | Number |
| :------------ | :------------ | :------------ | :------------ | :------------ | :------------ | :------------ | :------------ | -----: |
| 1             | 1             | 1             | 0             | 0             | 1             | 1             | 1             |    231 |
| 2<sup>7</sup> | 2<sup>6</sup> | 2<sup>5</sup> | 2<sup>4</sup> | 2<sup>3</sup> | 2<sup>2</sup> | 2<sup>1</sup> | 2<sup>0</sup> |

231 = 128 + 64 + 32 + 4 + 2 + 1

231 in binary is 1110 0111

# base-10 - Decimal

| 10<sup>2</sup> | 10<sup>1</sup> | 10<sup>0</sup> | Number |
| :------------- | :------------- | :------------- | -----: |
| 2              | 3              | 1              |    231 |

231 = 200 + 30 + 1

# base-16 - Hexadecimal

Encodes 1 byte (8 bits) into 2 hex characters.

| 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | A   | B   | C   | D   | E   | F   |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10  | 11  | 12  | 13  | 14  | 15  |

231 in hexadecimal is `E7`.

| 16<sup>1</sup> | 16<sup>0</sup> | Number |
| :------------- | :------------- | -----: |
| E              | 7              |    231 |

E7 = 14 x 16<sup>1</sup> + 7 x 16<sup>0</sup>  
E7 = 14 x 16 + 7 x 1  
E7 = 224 + 7 = 231  
E7 = 1110 0111

Hexadecimal numbers have either a `0x` prefix or an `h` suffix. So either `0xE7` or `E7h`.

# base-64

Encodes 3 source bytes (24 bits) into 4 base-64 characters.

I was curious how on EARTH base64 can convert 3 input bytes into 4 output bytes for just 33% space growth (whereas hex converts 1 input byte into 2 output bytes for 100% space growth). Why specifically 3 input bytes?

The answer is:

3 bytes = 3 x 8 bits = 24 bits.

Why that magic "24 bits" number? Well, base 64 represents the numbers 0 to 63. How are those represented in binary? With 000000 (0) to 111111 (63).

Bingo! Each base64 character represents 6 bits of input data using a single output byte (a single character such as "Z", etc).

So 24 bits (3 full bytes of input) / 6 bits (base64 alphabet) = 4 bytes of base64. That's it!

You may think "Why not base128 (7 bits of input = 8 bits of output), at just 14% size growth when encoding?". The answer for that is that base64 is the best we can find, since the lower 128 ASCII characters aren't all printable. Many are control characters such as NULL etc.

There are obviously ways to create other systems such as perhaps "base81" etc, since you can do anything you want if you create a custom encoding algorithm. But the beauty of base64 is how it encodes data so cleanly in chunks of 6 bits. So that encoding scheme became popular.
