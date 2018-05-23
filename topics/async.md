# Callback

A callback is a function that is to be executed after another function has finished executing — hence the name ‘call back’.

In JavaScript, functions are objects. Because of this, functions can take functions as arguments, and can be returned by other functions. Functions that do this are called **higher-order functions**. Any function that is passed as an argument is called a **callback function**.

```javascript
function foo(input, callback){
    console.log(input)
    callback()
}

// Anonymous callback. Logs Foo Bar.
foo("Foo", function(){
    console.log("Bar")
})

// Named callback. Logs Foo Baz.
foo("Foo", function(){
    baz("Baz")
})

function baz(input){
    console.log(input)
}

// Logs Baz Foo and "TypeError: callback is not a function
// This executes the baz function immediately.
foo("Foo", baz("Baz"))

// Works fine because it doesn't execute immediately.
// Must have no arguments. Logs Foo Qux.
foo("Foo", logQux)

function logQux(){
    console.log("Qux")
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

# Async/Await