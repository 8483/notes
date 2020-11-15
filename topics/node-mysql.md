```js
const express = require("express");
const util = require("util");

let databaseHandler = (pool) => {
    return (req, res, next) => {
        // let subodmain = req.subdomains[0];
        // req.subdomains is [] in deployment due to the reverse_proxy
        // req.decoded comes from authenticationHandler
        let database = req.decoded.database;

        // Get a connection from the main pool in app.js
        pool.getConnection((err, conn) => {
            if (err) {
                console.log(err);
                return;
            }
            // Change the database for the connection
            conn.changeUser({ database: database }, function (err) {
                if (err) {
                    console.log(err);
                    return;
                }

                // Promisify for Node.js async/await.
                const asyncQuery = util.promisify(conn.query).bind(conn);

                // Pass the modified query method down the chain as a property of req, avaiable via next()
                // req.query is used for queryStrings, so we use asyncQuery to avoid overwriting it
                // connection.release() is called in the routes
                req.asyncQuery = asyncQuery;
                req.connection = conn;
                req.tenant = database;

                next();
            });
        });
    };
};

module.exports = { databaseHandler };
```

```js
router.get("/api/companies/:companyId", async (req, res, next) => {
    try {
        let companyId = req.params.companyId;

        let query = `
            select *
            from company c
            where c.id = ?
        `;

        let rows = await req.query(query, [companyId]);
        let company = rows[0];

        res.status(200).send(company);
    } catch (err) {
        next(err);
    } finally {
        req.connection.release();
    }
});
```
