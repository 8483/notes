# Debugging

```javascript
// Write to console
console.log();

// Show DOM element
console.dir();

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

// Avoid if-else statements
function greaterThan(a, b) {
    console.assert(a > b, { message: "a is not greater than b", a: a, b: b });
}
greaterThan(2, 1); // a is not greater than b, a: 2, b: 1

// Heap size
console.memory;

// Display a table. Takes an array of objects.
console.table(array);
```
