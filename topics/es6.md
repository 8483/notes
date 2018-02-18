# Let / Const
These are a replacement for `var`.  
`let` has block scoping and `const` is immutable.  

### var
```javascript
function testVar(){
    var a = 30;
    if (true) {
        var a = 50;
        console.log(a);
    }
    console.log(a);
}
testVar(); // 50 50

for (var i = 0; i < 5; i++) {
    console.log(i); // 1, 2, 3, 4
}
console.log(i); // 5
```

### let
```javascript
function testLet(){
    let a = 30;
    if (true) {
        let a = 50;
        console.log(a);
    }
    console.log(a);
}
testLet(); // 50 30

for (let i = 0; i < 5; i++) {
    console.log(i); // 1, 2, 3, 4
}
console.log(i); // undefined
```

### const

```javascript
const a = 1;
console.log(a); // 1

a = 2;
console.log(a); // Error: Assignment to constant variable.
```

# Classes
```javascript
class User {
    // Called during instantiation.
    constructor(username, email, password){
        this.username = username,
        this.email = email,
        this.password = passwrod
    }

    // Callable without instantiation.
    static countUser(){
        console.log("There are 50 users");
    }

    // Regular method.
    register(){
        console.log(this.username + " is registered.");
    }
}

// Instantiation.
let bob = new User("Bob", "bob@email.com", "1234");
bob.register() // Bob is registered.

User.countUser(); // There are 50 users.
```

# Inheritance
```javascript
class Member extends User {
    constructor(username, email, password, memberPackage){
        super(username, email, password); // Avoids doing this.username = username...
        this.package = memberPackage; // Needs to be assigned since not in parent class.
    }

    getPackage(){
        console.log(this.username + " uses the " + this.package + " package." );
    }
}

let mike = new Member("Mike", "mike@email.com", "1234", "standard");
mike.getPackage(); // Mike uses the standard package.
mike.register(); // Mike is registered.
;
```

# Template Literals
Backticks allow multiline strings. Also, we can inject variables and functions with `${ var }`.

### Before
```javascript
var name = "John";
var template = "<h1>Hello, " + name + "</h1>" +
"<p>This is a simple template.</p>"
```

### After
```javascript
let name = "John";
let template = `<h1>Hello, ${name}</h1>
<p>This is a simple template.</p>`
```

# Strings
```javascript
let string = "Hello, I love Javascript."

string.startsWith("Hello"); // true
string.endsWith("Javascript"); // false, since there is no dot.
string.includes("love"); // true
```

# Numbers
```javascript
Number.isNaN("cat"); // true
Number.isInteger(1); // true
```

# Set
This is a collection of unique values. If we pass in an array with duplicates, we will get a collection with unique values back. A `weakSet` is a collection of **unique objects** only.
```javascript
var array = [1, 2, 3, 3];

var set = new Set(array); // Will have [1, 2, 3]
set.size; // 3
set.add("cat"); // [1, 2, 3, "cat"]
set.delete(2); // [1, 3, "cat"]
```

# Arrow Functions
It provides a shorter syntax, and more importantly, it binds `this` lexically, avoiding the usage of `that` or `self`.  

Until arrow functions, every new function defined its own `this` value. An arrow function **does not** have its own `this`; the `this` value of the enclosing execution context is used.

By not having a `this`, they are best suited for non-method functions. Using they as methods results in `undefined`.

```javascript
// Old
function Person() {
  var that = this;
  that.age = 0;

  setInterval(function growUp() {
    that.age++; // this.age would refer to growUp, instead of Person.
  }, 1000);
}

// New
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // this refers to the Person object.
  }, 1000);
}
```

# Promises
The promise object is used for deferred and asynchronous computations and it represents and operation that hasn't completed yet, but is expected in the future.  

Promises are obtained with `.then()`.

```javascript
// Immediately resolved.
let myPromise = Promise.resolve("Foo");
myPromise.then((res) => console.log(res);) // Foo

// Delayed
var myPromise = new Promise(function(resolve, reject){
    setTimeout(() => resolve(4), 2000); // Return 4 after 2 seconds.
});

myPromise.then((res) => {
    res += 3; // Wait for the 4 and add 3 to it.
    console.log(res); // 7 after 2 seconds.
});
```

### HTTP GET Promise
```javascript
function getData(method, url){
    return new Promise(function(resolve, reject){
        let xhr = new XMLHttpRequest();
        xhr.open(method, url);
        xhr.onload = function(){
            if (this.status >= 200 && this.staus <= 300) {
                resolve(xhr.response);
            } else {
                reject({
                    status: this.status,
                    statusText: xhr.statusText
                });
            }
        }
        xhr.onerror = function(){
            reject({
                status: this.status,
                statusText: xhr.statusText
            });
        };
        xhr.send();
    });
}

getData("GET", "http://jsonplaceholder.typicode.com/todos")
    .then(function(data){
        let todos = JSON.parse(data);
        let output = "";
        for (let todo of todos) {
            output += `
                <li>
                    <h3>${todo.title}</h3>
                    <p>${todo.item}</p>
                `;
        }
        document.getElementById("list").innerHTML = output;
    })
    .catch(function(err){
        console.log(err);
    })
```

# For loop
```javascript
let items = [1, 2, 3];
// Old
for (var i = 0; i < items.length; i++) {
    console.log(array[i]);
}

// New
for (let item of items){
    console.log(item);
}
```

# Generators
These are functions that can be paused and resumed. It can return i.e. yield values on each pause.

```javascript
function *generator(){
    console.log("Hello");
    yield "Yield 1 ran...";
    console.log("World");
    yield "Yield 2 ran...";
    return "Done";
}

let gen = generator(); // Set the generator to a variable to extract data.

console.log(gen.next().value); // Hello, Yield 1 ran...
console.log(gen.next().value); // World, Yield 2 ran...
console.log(gen.next().value); // Done...

for(let g of gen){
    console.log(g); // Hello, Yield 1 ran, World, Yield 2 ran. Ignores last.
}
```

# Destructuring
```javascript
// array
var foo = ['one', 'two', 'three', 'four'];

var [one, two, three] = foo;
console.log(one); // "one"
console.log(two); // "two"
console.log(three); // "three"

// object
var o = {p: 42, q: true};
var {p, q} = o;

console.log(p); // 42
console.log(q); // true
```
Electron example
```javascript
// Old
const electron = require("electron");
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;

// New
const electron = require("electron");
const {app, BrowserWindow} = electron;
```























