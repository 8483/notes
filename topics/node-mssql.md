**server.js**

```js
const express = require("express");
const http = require("http");
const sql = require("mssql");

const config = require("./config.js");
const middleware = require("./middleware.js");

let login = require("./api/auth/login");
let users = require("./api/auth/users");

let catalogue = require("./api/endpoints/catalogue");
let categories = require("./api/endpoints/categories");
let groups = require("./api/endpoints/groups");
let suppliers = require("./api/endpoints/suppliers");

const app = express();

app.use(express.json()); // Needed for POST and PATCH requests

app.all("/api/*", middleware.authenticationMiddleware);

// .use(middleware.requestsMiddleware)

app.use(login)
    .use(users)

    .use(catalogue)
    .use(categories)
    .use(groups)
    .use(suppliers)

    .use(middleware.errorMiddleware);

async function start() {
    // Initialize pool only once
    const connection = new sql.ConnectionPool(config.db);
    const pool = await connection.connect();

    // pool available in routes via let request = req.app.locals.pool.request();
    app.locals.pool = pool;
    app.locals.sql = sql;

    const server = http.createServer(app);

    server.listen(config.port, () => {
        console.log(`listening on port: ${config.port}`);
    });
}

start();
```

**users.js**

```js
const express = require("express");
const router = express.Router();

router.get("/api/users/:userId", async (req, res, next) => {
    try {
        let userId = req.params.userId;

        let request = req.app.locals.pool.request();
        let sql = req.app.locals.sql;

        request.input("userId", sql.Int, userId);

        let query = `
            select *
            from user u
            where u.id = @userId    
            ;
        `;

        let result = await request.query(query);

        let final = result.recordset[0];

        res.send(final);
    } catch (err) {
        next(err);
    }
});

module.exports = router;
```
