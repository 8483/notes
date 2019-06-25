# Install

```bash
npm i puppeteer --save

# This is needed for the dependencies
sudo apt install chromium-browser
```

# Scraping


# PDF

```javascript
const express = require("express");
const puppeteer = require('puppeteer')

var app = express();

// Allow requests from all domains and localhost
app.all("/*", function (req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header(
        "Access-Control-Allow-Headers",
        "X-Requested-With, Content-Type, Accept"
    );
    res.header("Access-Control-Allow-Methods", "POST, GET");
    next();
});

app.get('/pdf/:id', function (req, res) {

    let id = req.params.id;

    async function printPDF() {
        const browser = await puppeteer.launch({ headless: true, args: ['--no-sandbox', '--disable-setuid-sandbox'] });
        const page = await browser.newPage();
        await page.goto(`http://localhost:4000/${id}`, { waitUntil: 'networkidle0' });
        const pdf = await page.pdf({ format: 'A4' });

        await browser.close();
        return pdf
    }

    printPDF().then(pdf => {
        res.set({ 'Content-Type': 'application/pdf', 'Content-Length': pdf.length })
        res.send(pdf)
    })

});

app.get('/offer', function (req, res) {

    html = `<h1>Hello World!</h1>`
    res.send(html)

});

var server = app.listen(4000, function () {
    var host = server.address().address;
    var port = server.address().port;
    console.log("app listening at http://%s:%s", host, port);
});
```

