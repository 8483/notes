# Basics

Asynchronous Javascript is based on events. Every promise and observable library is based on them.

Synchronous code uses a `call stack`, a data structure keeping track of the program execution.

Asynchronous code uses an `event queue`, a list of functions handled in a FIFO manner, which is checked repeatedly by the `event loop`.

The event loop is a programming construct that waits for and dispatches functions in the `event queue` to the `call stack`.

```javascript
setTimeout(() => {
    console.log("I will happen later...");
}, 5000);
```

This doesn't execute after 5 seconds... It gets added to the `event queue` after 5 seconds.

In reality, **Javascript is not asynchronous at all**. It comes from the environment i.e. browsers and nodejs, specifically the Behavior Object Model (BOM).

window

-   Document Object Model (DOM)
    -   getElementById
-   Behavior Object Model (BOM)
    -   setTimeout
    -   XMLHttpRequest
    -   fetch
    -   history
-   ECMAScript
    -   Object
    -   Array
    -   Function

# AJAX

This is all done via vanilla Javascript, but it can be done via libraries such as JQuery, Axios, Superagent, Fetch API, Node HTTP...

```javascript
var xhr = new XMLHttpRequest();

xhr.open("GET", "http://www.api.com/data", true);

xhr.onload = function() {
    if (this.status == 200) {
        return JSON.parse(this.responseText);
    }
};

xhr.onerror = function() {
    console.log("error");
};

xhr.send();
```

Can also be done like this.

```javascript
var request = new XMLHttpRequest();

request.addEventListener("load", event => {
    console.log(event.target.responseText);
});

request.addEventListener("error", event => {
    console.error(event.target.responseText);
});

request.open("GET", "http://www.api.com/data");
request.send();
```

**Regular HTTP request**  
`Client` --- Request ---> `Server`  
`Client` <--- Response --- `Server`

**Ajax**  
`Client` --- JS Call ---> `Ajax Engine` --- XMLHttpRequest (XHR) ---> `Server`  
`Client` <--- HTML --- `Ajax Engine` <--- JSON --- `Server`

## XMLHttpRequest (XHR) Object

This API in the form of an object is provided by the browser's Javascript environment, and it has its own properties and methods which transfer data between the browser and a server.

```html
<button id="button">Get data</button>
```

```javascript
document.getElementById("button").addEventListener("click", loadData);
```

### Old way

We have to make sure we are on the `readyState 4`.

```javascript
// Create XHR Object
var xhr = new XMLHttpRequest(); // Can be named req

// Establish server connection - type, url/file, async
xhr.open("GET", "http://www.api.com/data", true); // readyState 1

xhr.onreadystatechange = function() {
    // readyState 1, 2, 3, 4
    if (this.readyState == 4 && this.status == 200) {
        return JSON.parse(this.responseText);
    }
};

// If it fails
xhr.onerror = function() {
    console.log("error");
};

// Send request
xhr.send();
```

**readyState values**

```
0. Request not initialized.
1. Server connection established.
2. Request received.
3. Processing request.
4. Request finished and response ready.
```

### New way

The difference from the old way is that `onload` will not run unless we are on `readyState 4`

```javascript
// Create XHR Object
var xhr = new XMLHttpRequest(); // Can be named req

// Establish server connection - type, url/file, async
xhr.open("GET", "http://www.api.com/data", true); // readyState 1

xhr.onload = function() {
    // readyState 1, 4
    if (this.status == 200) {
        return JSON.parse(this.responseText);
    }
};

// If it fails
xhr.onerror = function() {
    console.log("error");
};

// Send request
xhr.send();
```

### Loading

```javascript
xhr.onprogress = function() {
    // readyState 3
    // Show loading...
};
```

### POST

```html
<form method="POST" id="form">
    <input type="text" id="input">
    <input type="submit" value="submit">
</form>
```

```javascript
document.getElementById("form").addEventListener("submit", submitForm);

function submitForm(e) {
    e.preventDefault();

    var input = document.getElementById("input").value;
    var params = "input=" + input;

    var xhr = new XMLHttpRequest();

    xhr.open("POST", "http://www.api.com/data", true);

    xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded");

    xhr.onload = function() {
        // readyState 1, 4
        if (this.status == 200) {
            return JSON.parse(this.responseText);
        }
    };

    xhr.onerror = function() {
        console.log("error");
    };

    xhr.send(params);
}
```

# Fetch API

An AJAX library using promises, available by default in all modern browsers. A promise is like a placeholder for a response we are going to get in the future.

Fetch always returns a promise. Promise resolves to the response object. This response object has different helper methods like response.json(), response.text(), response.blob() etc.

Fetch also takes a second parameter, which is a configuration object.

## GET

```javascript
// ES5
function getData() {
    fetch("http://www.api.com/data") // Return response promise.
        .then(function(res) {
            return res.json(); // Return data.
        })
        .then(function(data) {
            return data; // Do something with data.
        })
        .catch(function(err) {
            return err; // Catch error.
        });
}

// ES6
function getText() {
    fetch("http://www.api.com/data") // Return response promise.
        .then(res => res.json()) // Return data.
        .then(data => data) // Do something with data.
        .catch(err => err); // Catch error.
}
```

## POST

```javascript
function postData() {
    fetch("http://www.api.com/data", {
        method: "POST",
        headers: {
            Accept: "application/json",
            "Content-type": "application/json"
        },
        body: JSON.stringify({ title: title, body: body }) // Payload
    }) // Return response promise.
        .then(res => res.json()) // Return data.
        .then(data => data) // Do something with data.
        .catch(err => err); // Catch error.
}
```

# Callbacks

A callback is a function that is to be executed after another function has finished executing — hence the name ‘call back’.

In JavaScript, functions are objects. Because of this, functions can take functions as arguments, and can be returned by other functions. Functions that do this are called **higher-order functions**. Any function that is passed as an argument is called a **callback function**.

```javascript
function foo(input, callback) {
    console.log(input);
    callback();
}

// Anonymous callback. Logs Foo Bar.
foo("Foo", function() {
    console.log("Bar");
});
```

Passing named functions.

```javascript
function baz(input) {
    console.log(input);
}

// This executes the baz function immediately.
// Logs Baz Foo and "TypeError: callback is not a function
foo("Foo", baz("Baz"));

// Named callback. Logs Foo Baz.
foo("Foo", function() {
    baz("Baz");
});

// Works fine because it doesn't execute immediately.
// Must have no arguments. Logs Foo Qux.
foo("Foo", logQux);

function logQux() {
    console.log("Qux");
}
```

Another example...

```javascript
function callbackSandwich(callbackFunction) {
    console.log("Top piece of bread.");
    callbackFunction();
    console.log("Bottom piece of bread.");
}

// We pass in an anonymous function, to be called inside.
callbackSandwich(function() {
    console.log("Slice of cheese.");
});
```

## AJAX with callback

```javascript
var request = new XMLHttpRequest();

request.addEventListener("load", event => {
    console.log(event.target.responseText);
});

request.open("GET", "http://www.api.com/data");
request.send();
```

Can be refactored into...

```javascript
function ajax(method, url, callback) {
    var request = new XMLHttpRequest();
    request.addEventListener("load", callback);
    request.open(method, url);
    request.send();
}

ajax("GET", "http://www.api.com/data", event => {
    console.log("SUCCESS", event.target.responseText);
});
```

Which is pretty much what the ajax libraries do.

# Promises

The `Promise()` constructor takes a function, which takes the `resolve` and `reject` functions.

```javascript
new Promise((resolve, reject) => {
    if (goodThingsHappen) {
        resolve(goodThings); // passed to .then()
    } else {
        reject(reasonsForFailing);
    }
});
```

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

## Fetch under the hood with Promises

```javascript
function fetch(method, url) {
    return new Promise(function(resolve, reject) {
        let xhr = new XMLHttpRequest();
        xhr.open(method, url);
        xhr.onload = function() {
            if (this.status >= 200 && this.staus <= 300) {
                resolve(xhr.response);
            } else {
                reject({
                    status: this.status,
                    statusText: xhr.statusText
                });
            }
        };
        xhr.onerror = function() {
            reject({
                status: this.status,
                statusText: xhr.statusText
            });
        };
        xhr.send();
    });
}

fetch("GET", "http://jsonplaceholder.typicode.com/todos")
    .then(function(data) {
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
    .catch(function(err) {
        console.log(err);
    });
```

## Promise All

Takes an array of promises and executes if they are all successful.

```javascript
Promise.all([
    fetch("/api/endpoint"),
    fetch("/api/another-endpoint"),
    fetch("/api/yet-another-endpoint")
])
    .then(responses => {
        // array of responses
        console.log(responses[0]); // users
        console.log(responses[1]); // products
        console.log(responses[2]); // clients
    })
    .catch(err => {
        console.log(err);
    });
```

## Chaining

```javascript
let futureNumber = Promise.resolve(2); // 2

futureNumber
    .then(n => n + 1) // 3
    .then(n => n * 2) // 6
    .then(n => Math.pow(n, 2)) // 36
    .then(n => console.log(n)); // 36

futureNumber.then(n => console.log(n)); //2
```

# Async/Await

Async/Await enables us to write asynchronous code in a synchronous fashion. Under the hood, it’s just syntactic sugar using generators and yield statements to “pause” execution. 

In other words, async functions can “pull out” the value of a Promise even though it’s nested inside a callback function, giving us the ability to assign it to a variable.

```javascript
async function foo() {
    try{
        let res = await fetch("https://jsonplaceholder.typicode.com/todos");
        let data = await res.json()
        console.log(data);
    } catch (err){
        console.log(err)
    }
};

foo();

// Fetch API comparison

function foo() {
    fetch("https://jsonplaceholder.typicode.com/todos")
        .then(res => res.json())
        .then(data => console.log(data))
        .catch(err => console.log(err));
}
```

Under the hood it looks like...

```javascript
const request = url => {
    ajax("GET", url).then(response => it.next(JSON.parse(response)));
};

function* main() {
    const result = yield fetch("http://api.com/users/1");
    const data = yield result.json();
    console.log(data);
}

const it = main();
it.next();
```

### Combination Example
```javascript
async function getDataOne() {
    let res = await fetch("https://jsonplaceholder.typicode.com/todos/1");
    let data = await res.json()
    return data;
}

async function getDataTwo() {
    let res = await fetch("https://jsonplaceholder.typicode.com/todos/2");
    let data = await res.json()
    return data;
}

async function getBoth() {
    var resultOne = await getDataOne();
    var resultTwo = await getDataTwo();
    console.log(resultOne, resultTwo)
}

getBoth();

// Compared to this
function foo() {
    fetch("https://jsonplaceholder.typicode.com/todos/1")
    .then(res => res.json())
    .then(resultOne => {

        fetch("https://jsonplaceholder.typicode.com/todos/2")
        .then(res => res.json())
        .then(resultTwo => {

            console.log(resultOne, resultTwo)
            
        })
        .catch(err => console.log(err));

    })
    .catch(err => console.log(err));
}

foo()
```