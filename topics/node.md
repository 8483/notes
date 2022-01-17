# Install

### nvm

It allows using different node versions for different projects.

```bash
# Download and install
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
```

**NOTE: Restart your terminal for your changes to take effect.**

```bash
# Check if installed
nvm --version

# Install latest nodejs
nvm install node

# List versions
nvm ls

# Use latest version
nvm use node

# Use specific version
nvm use 10.16.3
```

```bash
# Install specific version
nvm install 8.16.2
```

**NOTE:** If you get this error: `/usr/bin/env: 'node': No such file or directory`

```bash
# /home/user/.nvm/versions/node/v17.2.0/bin/node
sudo ln -s "$(which node)" /usr/local/bin/node
```

### apt-get

**If you node with apt-get, you'll end up with v10.19.0, which is the latest version in the ubuntu app store, but it's not the latest released version of NodeJS.**

```bash
# remove old version
sudo apt-get purge nodejs
sudo apt-get autoremove
sudo rm -rf /usr/local/bin/node
sudo rm -rf /usr/local/lib/node_modules/npm/

# install new one
sudo apt-get update
sudo apt-get install nodejs

# Check install
node -v
npm -v
```

# Import / Export

| CommonJS        | ES6    |
| --------------- | ------ |
| require         | import |
| modules.exports | export |

You can't selectively load only the pieces you need with require but with imports, you can selectively load only the pieces you need. That can save memory.

Loading is synchronous (step by step) for require on the other hand import can be asynchronous(without waiting for previous import) so it can perform a little better than require.

-   **CommonJS (CJS) format**. Used in Node.js and uses `require` and `module.exports` to define dependencies and modules. _The npm ecosystem is built upon this format_. `exports = module.exports`

```javascript
// lib.js

// Export the function
function sayHello() {
    console.log("Hello");
}

// Do not export the function
function somePrivateFunction() {
    // ...
}

module.exports.sayHello = sayHello;
```

```javascript
let sayHello = require("./lib").sayHello;

sayHello();
// => Hello
```

-   **ES Module (ESM) format**. As of ES6 (ES2015), JavaScript supports a native module format. It uses an `export` keyword to export a module’s public API and an `import` keyword to import it.

```javascript
// lib.js

// Export the function
export function sayHello() {
    console.log("Hello");
}

// Do not export the function
function somePrivateFunction() {
    // ...
}
```

```javascript
import { sayHello } from "./lib";
// import * as lib from './lib';

sayHello();
// => Hello
```

# Modules

Instead of writing all the code in one giant file, we can split the code into multiple files called `modules`. Only things that are highly related should go in a module.

This increases maintainability, code reuse and abstraction (blackbox).

**CommonJS** (old) - Synchronous

```javascript
// foo.js module
function foo() {
    return "bar";
}
module.exports.foo = foo;

// index.js use
const { foo } = require("foo");
```

**ES6** - Must use Babel, can be asynchronous

```javascript
// foo.js module
export function foo() {
    return "bar";
}

// index.js use
import { foo } from "foo";
```

Every file in Node is considered a module. Everything declared inside is scoped to the file i.e. they are private.

Node does not give access to the global scope. In order to use the content of a module, we need to export it i.e. make it public.

```javascript
var message = "hello";
console.log(global.message); // undefined
```

## ES6 Modules

A proper format, unlike the CommonJS convention.

```javascript
// add.js
export function add(a, b) {
    return a + b;
}

// index.js
import { add } from "./add";
```

Modules are exported with `export` and imported with `import`. We can export one or more objects from a module.

There are `default` and `named` exports. We use a default export if there is a single object we want to export.

**module**

```javascript
import { Person } from "./person";

// Named export
export function promote() {}

// Default export
export default class Teacher extends Person {
    constructor(name, degree) {
        super(name);
        this.degree = degree;
    }

    teach() {
        console.log("teach");
    }
}
```

**import**

```javascript
import Teacher, { promote } from "./teacher";

// Default -> import ... from "";
// Named -> import { ... } from "";

const teacher = new Teacher("John", "MSc");
teacher.teach();
```

### Browsers

Must include the `.js` file extension during import.

**circle.js**

```javascript
// Implementation detail, not exported
const _radius = new WeakMap(); // private property

// Public interface i.e. exported part
export class Circle {
    constructor(radius) {
        _radius.set(this, radius);
    }

    draw() {
        console.log("Circle with radius" + _radius.get(this));
    }
}
```

**index.js**

```javascript
import { Circle } from "./circle.js"; // Must include the .js file extension

const c = new Circle(10);
c.draw(); // Circle with radius 10
```

**index.html**

We need to add the module type in order to avoid the `Uncaught SyntaxError: Unexpected token {` error, caused by the `import` curly brace.

```html
<script type="module" src="index.js"></script>
```

## Old formats

Conventions/syntax for defining modules. ES6 natively supports them.

-   **CommonJS** - Loads files synchronously.

Since ES5 doesn't support modules, developers came up with different syntaxes to define them. These are only used in legacy applications.

-   **AMD** (Browser) - Asynchronous Module Definition.
-   **UMD** (Browser / Node)- Universal Module Definition.

### CommonJS (Node)

Two problems:

1. Browsers cannot load files synchronously. This is solved via bundling a huge file including everything, even unused things.
2. JS engine cannot tell what a module exports until it runs it.

```javascript
// add.js
function add(a, b) {
    return a + b;
}
module.exports = add;

// index.js
const add = require("./add"); // Loads synchronously
add(2, 3); // 5
```

CommonJS defines the:

-   `module.exports` for exporting modules. It represents the object that is exported from a module.
    -   `module` refers to the current module (file).
    -   `exports` is an object and property of `module`.
-   `require("./module")` for importing modules.

#### Single class export

When we import the `foo.js` module, we directly get the `Foo` class.

```javascript
class Foo {}

module.exports = Foo;
```

#### Multiple class exports

We can import the `exports` objects and access its properties.

```javascript
class Foo {}
class Bar {}

module.exports.Foo = Foo;
module.exports.Bar = Bar;
```

#### Example

**circle.js**

```javascript
// Implementation detail, not exported
const _radius = new WeakMap(); // private property

// Public interface i.e. exported part
class Circle {
    constructor(radius) {
        _radius.set(this, radius);
    }

    draw() {
        console.log("Circle with radius" + _radius.get(this));
    }
}

module.exports = Circle;
```

**index.js**

```javascript
const Circle = require("./circle");

const c = new Circle(10);
c.draw(); // Circle with radius 10
```
