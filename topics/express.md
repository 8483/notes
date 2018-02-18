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


