https://www.youtube.com/watch?v=J1f5b4vcxCQ

> A piece of code using another piece of code that is passed in, instead of used directly.

The act of passing things to be used is called `injection` i.e. We inject the dependent code into the code that uses it.

**Before**

```js
export class Storage(){
    private database: Database

    constructor(){
        this.database = New Database("db.sqlite");

        this.database.run(`
            CREATE TABLE IF NOT EXISTS user (
                id INTEGER PRIMARY KEY,
                username TEXT,
                group_id INTEGER,
                creation_time TIMESTAMP
            )
        `);
    }
}
```

**After**

```js
export class Storage(){
    private database: Database

    constructor(database: Database){
        this.database = database;

        this.database.run(`
            CREATE TABLE IF NOT EXISTS user (
                id INTEGER PRIMARY KEY,
                username TEXT,
                group_id INTEGER,
                creation_time TIMESTAMP
            )
        `);
    }
}
```
