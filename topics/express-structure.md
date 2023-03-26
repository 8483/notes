# Simple

```js
router.get("/api/products/:productId", async (req, res) => {
    try {
        let { productId } = req.params;

        const pool = await poolPromise;
        const request = await pool.request();

        let query = `
            select *
            from product p
            where p.id = @productId
        `;

        request.input("productId", sql.Int, productId);

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

# Layered

This is useful because the functions are unit testable.

**/api/products.js**

```js
const productsService = require("../services/products-service");

const service = new productsService();

router.get("/api/products/:productId", async (req, res) => {
    try {
        let { productId } = req.params;
        let { product } = await service.getProductById(productId);

        res.send(product);
    } catch (err) {
        console.log("Error: " + err);
    } finally {
        sql.close();
    }
});
```

**/services/products-service.js**

```js
const ProductsRepository = require("../repository/products-repository")
const { someBusinessLogic } = require("../utils")

class ProductsService(){
    constructor(){
        this.repository = new ProductsRepository();
    }

    async getProductById(productId){
        try {
            let product = await this.repository.findProductById(productId);

            return someBusinessLogic(product);

        } catch (err) {
            throw new Error(err)
        }
    }

}
```

**/repository/products-repository.js**

```js
const ProductsRepository = require("mssql")

class ProductsRepository(){
    constructor(){
        this.repository = new ProductsRepository();
    }

    async findProductById(productId){
        try {
            const pool = await poolPromise;
            const request = await pool.request();

            let query = `
                select *
                from product p
                where p.id = @productId
            `;

            request.input("productId", sql.Int, productId);

            let result = await request.query(query);

            let product = result.recordset;

            return product;

        } catch (err) {
            throw new Error(err)
        }
    }

}
```

# chatGPT

**server**

```js
const express = require("express");
const http = require("http");
const sql = require("mssql");

const config = require("./config.js");
const middleware = require("./middleware.js");

const authRoutes = require("./api/auth");
const endpointsRoutes = require("./api/endpoints");

const app = express();
app.use(express.json()); // Needed for POST and PATCH requests

app.all("/api/*", middleware.authenticationMiddleware);

app.use(authRoutes);
app.use(endpointsRoutes);

app.use(middleware.errorMiddleware);

async function start() {
    // Initialize pool only once
    const connectionPool = new sql.ConnectionPool(config.db);
    const pool = await connectionPool.connect();

    // pool available in routes via req.app.locals.pool.request();
    app.locals.pool = pool;

    const server = http.createServer(app);

    server.listen(config.port, () => {
        console.log(`listening on port: ${config.port}`);
    });
}

start();
```

**endpointsRoutes**

```js
const express = require("express");
const router = express.Router();

const middleware = require("../middleware.js");
const catalogueHandlers = require("./catalogueHandlers.js");
const categoriesHandlers = require("./categoriesHandlers.js");
const groupsHandlers = require("./groupsHandlers.js");
const suppliersHandlers = require("./suppliersHandlers.js");

// Middleware for all endpoints in this file
router.use(middleware.requestsMiddleware);

// Catalogue resource endpoints
router.get("/catalogue", catalogueHandlers.getAll);
router.post("/catalogue", catalogueHandlers.create);
router.get("/catalogue/:id", catalogueHandlers.getById);
router.patch("/catalogue/:id", catalogueHandlers.updateById);
router.delete("/catalogue/:id", catalogueHandlers.deleteById);

// Categories resource endpoints
router.get("/categories", categoriesHandlers.getAll);
router.post("/categories", categoriesHandlers.create);
router.get("/categories/:id", categoriesHandlers.getById);
router.patch("/categories/:id", categoriesHandlers.updateById);
router.delete("/categories/:id", categoriesHandlers.deleteById);

// Groups resource endpoints
router.get("/groups", groupsHandlers.getAll);
router.post("/groups", groupsHandlers.create);
router.get("/groups/:id", groupsHandlers.getById);
router.patch("/groups/:id", groupsHandlers.updateById);
router.delete("/groups/:id", groupsHandlers.deleteById);

// Suppliers resource endpoints
router.get("/suppliers", suppliersHandlers.getAll);
router.post("/suppliers", suppliersHandlers.create);
router.get("/suppliers/:id", suppliersHandlers.getById);
router.patch("/suppliers/:id", suppliersHandlers.updateById);
router.delete("/suppliers/:id", suppliersHandlers.deleteById);

// Middleware for all endpoints in this file
router.use(middleware.errorMiddleware);

module.exports = router;
```

**categoriesHandlers**

```js
const { sql } = require("mssql");

// Get all categories
async function getAll(req, res) {
    try {
        const request = req.app.locals.pool.request();
        const result = await request.query("SELECT * FROM categories");
        res.status(200).json(result.recordset);
    } catch (err) {
        console.error(err);
        res.status(500).send("Internal server error");
    }
}

// Get a category by id
async function getById(req, res) {
    try {
        const categoryId = req.params.id;
        const request = req.app.locals.pool.request();
        const result = await request.input("id", sql.Int, categoryId).query("SELECT * FROM categories WHERE id = @id");
        if (result.recordset.length === 0) {
            res.status(404).send(`Category with id ${categoryId} not found`);
        } else {
            res.status(200).json(result.recordset[0]);
        }
    } catch (err) {
        console.error(err);
        res.status(500).send("Internal server error");
    }
}

// Create a new category
async function create(req, res) {
    try {
        const { name, description } = req.body;
        const request = req.app.locals.pool.request();
        const result = await request.input("name", sql.NVarChar, name).input("description", sql.NVarChar, description).query("INSERT INTO categories (name, description) OUTPUT INSERTED.* VALUES (@name, @description)");
        res.status(201).json(result.recordset[0]);
    } catch (err) {
        console.error(err);
        res.status(500).send("Internal server error");
    }
}

// Update a category by id
async function updateById(req, res) {
    try {
        const categoryId = req.params.id;
        const { name, description } = req.body;
        const request = req.app.locals.pool.request();
        const result = await request.input("id", sql.Int, categoryId).input("name", sql.NVarChar, name).input("description", sql.NVarChar, description).query("UPDATE categories SET name = @name, description = @description OUTPUT INSERTED.* WHERE id = @id");
        if (result.recordset.length === 0) {
            res.status(404).send(`Category with id ${categoryId} not found`);
        } else {
            res.status(200).json(result.recordset[0]);
        }
    } catch (err) {
        console.error(err);
        res.status(500).send("Internal server error");
    }
}

// Delete a category by id
async function deleteById(req, res) {
    try {
        const categoryId = req.params.id;
        const request = req.app.locals.pool.request();
        const result = await request.input("id", sql.Int, categoryId).query("DELETE FROM categories OUTPUT DELETED.* WHERE id = @id");
        if (result.recordset.length === 0) {
            res.status(404).send(`Category with id ${categoryId} not found`);
        } else {
            res.status(200).json(result.recordset[0]);
        }
    } catch (err) {
        console.error(err);
        res.status(500).send("Internal server error");
    }
}

module.exports = {
    getAll,
    getById,
    create,
    updateById,
    deleteById,
};
```
