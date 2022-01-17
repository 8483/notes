# AWS Lambda/Serverless

```bash
# Install
npm install -g serverless

# Initialize (Setup AWS credentials)
serverless

# Deploy code
serverless deploy

# Call API
curl https://pv2vfelg5j.execute-api.us-east-1.amazonaws.com/dev/api
```

# Code

```bash
npm install express aws-serverless-express
```

**api.js**

```js
const express = require("express");

router.get("/", (req, res) => {
    res.status(200).json({ foo: "bar" });
});

module.exports = router;
```

**app.js**

```js
const express = require("express");
const cors = require("cors");

let api = require("./api");

const app = express();

app
    .use(cors());
    .use(express.json())
    .use(api);

module.exports = app;
```

**handler.js**

```js
const awsServerlessExpress = require("aws-serverless-express");
const app = require("./app");

const server = awsServerlessExpress.createServer(app);

exports.handler = (event, context) => {
    return awsServerlessExpress.proxy(server, event, context);
};
```

**serverless.yml**

```yml
service: api

provider:
    name: aws
    runtime: nodejs12.x

functions:
    app-api:
        handler: handler.handler
        events:
            - http:
                  path: /
                  method: get
                  cors: true
```
