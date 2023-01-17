# Hoisting

Both variable and function declarations are hoisted to the top on code execution, meaning that their order is irrelevant i.e functions can be called before they are declared.

# Variables

```javascript
var a;     // Regular.
let c;     // Block scoped.
const b;   // Immutable.
```

# Functions

Functions are first class objects - a function is a regular object of type `function`. The function object type has a constructor: `Function`.

There are several ways to declare a function.

The difference is how the function interacts with the external components (the outer scope, the enclosing context, object that owns the method, etc) and the invocation type (regular function invocation, method invocation, constructor call, etc).

## Function declaration

**Hoisted**. Available immediately after parsing, before any code is executed.

The function declaration creates a variable in the current scope with the identifier equal to function name. This variable holds the function object.

```javascript
function foo() {}
foo();
```

Use them when a function expression is not appropriate or when it is important that that a function is hoisted.

## Function expression

**Not hoisted.** Available only after the variable assignment is executed.

```javascript
// Named
let bar = function foo() {};
bar();
foo(); // undefined
```

Use them when you are doing recursion or want to see the function name in the debugger.

```javascript
// Anonymous
let foo = function () {};
foo();

let bar = foo();
bar(); // Error: not a function.
```

Use them when you want to pass a function as an argument to another function or you want to form a closure.

## IIFE - immediately Invoked Function Expression

```javascript
(function () {
    // ...
})();
```

Use them for the module pattern.

## ES6

Binds `this` automatically.

```javascript
let foo = () => {};
```

Use them when you want to lexically bind the `this` value.

## Function constructor (Avoid this)

```javascript
let foo = new Function();
```

## Other

-   Use function declaration generators `function* foo(){}` when you want to exit and then re-enter a function.
-   Use function expression generators `let foo = function* [name](){}` when you want to exit and then re-enter a nested function.

## Function parameters vs arguments

An argument is the value supplied to the parameter.

```javascript
function foo(bar) {
    // bar is a parameter
    console.log(bar);
}

foo("baz"); // baz is an argument.
```

# Useful

## Truthy / Falsy

Strings with at least one letter and numbers larger than zero are `truthy`.

```javascript
console.log(true && "foo"); // foo
console.log(true && "foo" && 1); // 1
```
