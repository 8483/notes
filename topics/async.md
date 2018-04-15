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