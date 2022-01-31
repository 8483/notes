# Hashing

```js
const SHA256 = require("crypto-js/sha256");

console.log(SHA256("pass").toString());

// d74ff0ee8da3b9806b18c877dbf29bbde50b5bd8e4dad7a3a725000feb82e8f1

console.log(SHA256("pass"));

/*
    {
    words: [
        -682626834,
        -1918649984,
        1796786295,
        -604857411,
        -452240424,
        -455419997,
        -1490747377,
        -343742223
    ],
    sigBytes: 32
    }
*/
```

# Encryption

**encrypt**

```js
const CryptoJS = require("crypto-js");
const readline = require("readline");

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout,
});

rl.question("Enter string for encryption: ", (string) => {
    rl.question("Enter secret: ", (secret) => {
        let encrypted = CryptoJS.AES.encrypt(string, secret).toString();
        console.log(`Encrypted string: ${encrypted}`);
        rl.close();
    });
});
```

**decrypt**

```js
const CryptoJS = require("crypto-js");
const readline = require("readline");

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout,
});

rl.question("Enter string for decryption: ", (string) => {
    rl.question("Enter secret: ", (secret) => {
        let bytes = CryptoJS.AES.decrypt(string, secret);
        let decrypted = bytes.toString(CryptoJS.enc.Utf8);
        console.log(`Decrypted string: ${decrypted}`);
        rl.close();
    });
});
```
