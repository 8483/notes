# Log

```js
// Write to console
console.log();

// Show DOM element
console.dir();

// Display a table. Takes an array of objects.
console.table(array);
```

# Execution

```js
// Display the call stack of a function
console.trace();

// Track execution time
console.time("point"); // undefined
console.timeEnd("point"); // point: 1337.42 ms

// Count the number of executions
console.count("foo"); // foo: 1
console.count("foo"); // foo: 2
console.countReset("foo"); // undefined
console.count("foo"); // foo: 1
```

# Memory

```js
// Heap size
console.memory;

// Show memory usage
setInterval(() => {
    const used = process.memoryUsage().heapUsed / 1024 / 1024;
    console.log(`Script uses ${Math.round(used * 100) / 100} MB`);
}, 1000);
```

# Test

```js
// Avoid if-else statements
function greaterThan(a, b) {
    console.assert(a > b, { message: "a is not greater than b", a: a, b: b });
}

greaterThan(2, 1); // a is not greater than b, a: 2, b: 1
```
