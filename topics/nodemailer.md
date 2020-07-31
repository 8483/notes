```js
const nodemailer = require("nodemailer");

(async function () {
    try {
        let mailOptions = {
            from: '"Vendor Name" <info@vendor.com>',
            to: ["info@clientone.com", "contact@clienttwo.com"],
            subject: `Message from vendor`,
            // text: // plain text body
            html: `
                    <h1>Some Title</h1>
                    <br><br>
                    <span>Some text</span>
                `,
        };

        // create reusable transporter object using the default SMTP transport
        let transporter = nodemailer.createTransport({
            name: "subdomain.vendor.com", // needed to send to gmail, yahoo
            host: "mail.vendor.com",
            port: 465,
            secure: true, // true for 465, false for other ports
            auth: {
                user: "info@vendor.com", // generated ethereal user
                pass: "emailPassword", // generated ethereal password
            },
        });

        // send mail with defined transport object
        transporter.sendMail(mailOptions, (error, info) => {
            if (error) {
                console.log(error);
                return;
            }

            // If sent
            console.log(info);

            // Preview only available when sending through an Ethereal account
            // console.log("Preview URL: %s", nodemailer.getTestMessageUrl(info));
        });
    } catch (err) {
        console.log("Error: " + err);
    }
})();
```
