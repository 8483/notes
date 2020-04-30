# Middleware

*A stack of functions, executed before the final request handler is made.*

**Middleware functions** are functions with access to the `req` and `res` objects. They can... 
- Execute any code.
- Make changes to the `req` and `res` objects.
- End the response cycle.
- Call the next middleware in the stack, by using `next()`.

The functions do something, then pass the results to the next middleware in the chain.

Express has built-in ones, but we can also make custom ones.

```js
// Logggin middleware
function log(req, res, next){
    console.log(new Date(), req.method, req.url);
    next() // MUST USE THIS
}
```

```js
// This will make a log before EACH request in the app.
app.use(log); 

app.get("/", (req, res) => {
    res.write("Hello World!");
    res.end;
})
```

```js
// This will make a log before THIS request ONLY.
app.get("/", log, (req, res) => {
    res.write("Hello World!");
    res.end;
})
```

### Authentication example

```js
app.post("/upload", auth.isAuthenticated(), controller.upload);
```

!["Middleware"](../pics/express/express_middleware.jpg)




# JSON

JSON is used to transfer data via a string.  

An object from one programming language can be transferred to another by encoding it in JSON and then decoding it, even though they cannot understand each others' objects.  

```Javascript
// Javascript object literal.
var personObject = {
    firstName:"John",
    lastName:"Doe",
    age:50,
    eyeColor:"blue"
};

// Convert object to JSON.
var personJSON = JSON.stringify(personObject);

// personJSON.
{
    "firstName":"John",
    "lastName":"Doe",
    "age":50,
    "eyeColor":"blue"
}

// Convert JSON to object.
JSON.stringify(personJSON);

```

# MSSQL

```Javascript
// Callback
sql.connect(db, function (err) {
    if (err) console.log(err);
    var request = new sql.Request();
    request.query("SELECT * FROM product", function (err, recordset) {
        if (err) console.log(err)

        let rows = recordset.recordsets[0];
        res.send(rows);
        sql.close(); // Important
    });
});

// Promises
new sql.ConnectionPool(db).connect().then(pool => {
    return pool.request().query("SELECT * FROM product")
}).then(result => {
    let rows = result.recordset
    res.setHeader('Access-Control-Allow-Origin', '*')
    res.status(200).json(rows);
    sql.close();
}).catch(err => {
    res.status(500).send({ message: "${err}"})
    sql.close();
});
```


