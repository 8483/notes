```js
const sql = require("mssql");
const db = require("./db.js");

const poolPromise = new sql.ConnectionPool(db)
    .connect()
    .then((pool) => {
        // console.log("Connected to MSSQL");
        return pool;
    })
    .catch((err) => console.log("Database Connection Failed! Bad Config: ", err));

module.exports = {
    sql,
    poolPromise,
};
```

```js
const { sql, poolPromise } = require("../../pool");

router.get("/api/companies/:companyId", async (req, res) => {
    try {
        const pool = await poolPromise;
        const request = await pool.request();

        let query = `
            select * 
            from company c
            where c.id = @companyId
        `;

        request.input("companyId", sql.Int, companyId);

        let result = await request.query(query);

        let final = result.recordset;

        res.send(final);
    } catch (err) {
        console.log("Error: " + err);
    } finally {
        sql.close();
    }
});
```
