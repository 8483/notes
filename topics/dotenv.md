In order to use different parameters in our app depending on the enviroment such as...

-   Server ports
-   Database credentials
-   API keys

...we can declare environment variables.

We do this by having a `.env` file to store the variables.

**NEVER COMMIT THIS FILE. PUT IT IN .gitignore!**

```
DBNAME=database
DBUSER=root
DBPASSWORD=root
```

Which we can read with the `dotenv` package like so.

```js
// config.js
const dotenv = require("dotenv").config({
    path: `${__dirname}/.env`,
});

const database = process.env.DBNAME;
const user = process.env.DBUSER;
const password = process.env.DBPASSWORD;

module.exports = {
    db: {
        server: "localhost",
        database: database,
        user: user,
        password: password,
    },
};
```

**NOTE:** `__dirname` is required because the variables are `undefined` when accessing the `.env` file outside the root folder.

We can then have one `.env` file for development in the local machine, and one for production on the live server. This way we can just set the variables and not touch the code.

**NEVER COMMIT THIS FILE. PUT IT IN .gitignore!**

We can also pass in the variables without a `.env` file and run the script.

```
DBNAME=database DBUSER=root DBPASSWORD=root node server.js
```
