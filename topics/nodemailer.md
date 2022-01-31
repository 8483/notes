```js
const nodemailer = require("nodemailer");

async function sendMail() {
    try {
        let mailOptions = {
            from: '"Vendor Name" <info@vendor.com>',
            to: ["info@clientOne.com", "contact@clientTwo.com"],
            subject: `Message from vendor`,
            // text: // plain text body
            html: `
                <h1>Some Title</h1>
                <br><br>
                <p>Some text</p>
            `,
        };

        // create reusable transporter object using the default SMTP transport
        let transporter = nodemailer.createTransport({
            name: "subdomain.vendor.com", // needed to send to gmail, yahoo
            host: "mail.vendor.com", // email provider SMTP server
            port: 465,
            secure: true, // true for 465, false for other ports
            auth: {
                user: "info@vendor.com", // has to be the same as mailOptions.from
                pass: "emailPassword",
            },
        });

        // send mail with defined transport object
        // transporter.sendMail does not return a promise, it uses a callback function.
        transporter.sendMail(mailOptions, async (error, info) => {
            if (error) {
                res.send({
                    success: false,
                    message: "Sending failed.",
                });
            } else {
                res.send({
                    success: true,
                    message: "Sent mail.",
                });
            }
        });
    } catch (err) {
        console.log("Error: " + err);
    }
}
```

# Promise response

```js
return new Promise((resolve, reject) => {
    transporter.sendMail(mailOptions, (error, info) => {
        if (error) {
            reject({
                success: false,
                type: "error",
                message: "Sending failed.",
            });
        } else {
            resolve({
                success: true,
                type: "success",
                message: "Sent mail.",
            });
        }
    });
});
```
