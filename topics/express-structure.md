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
