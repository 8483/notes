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
