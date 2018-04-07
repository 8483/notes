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

```javascript
// Function declaration (Preferred)
// Available immediately after parsing, before any code is executed.
// The function declaration creates a variable in the current scope with the identifier equal to function name. This variable holds the function object.
function foo(){};
foo();

// Function expression.
// Available only after the variable assignment is executed.
var foo = function(){};
foo();
var bar = foo();
bar(); // Error: not a function.

// Named function expression.
var bar = function foo(){};
bar();
foo(); // undefined

// IIFE - immediately Invoked Function Expression
(function(){})();

// ES6
// Binds .this automatically.
var foo = () => {};

// Function constructor (Avoid this)
var foo = new Function();
```

### Uses
- Use function declarations `function foo(){}` when a function expression is not appropriate or when it is important that that a function is hoisted.
- Use anonymous function expressions `var foo = function(){}` when you want to pass a function as an argument to another function or you want to form a closure.
- Use named function expressions `var foo = function bar(){}` when you are doing recursion or want to see the function name in the debugger.
- Use arrow function expressions `() => {}` when you want to lexically bind the `this` value.
- Use function declaration generators `function* foo(){}` when you want to exit and then re-enter a function.
- Use function expression generators `var foo = function* [name](){}` when you want to exit and then re-enter a nested function.
- Use immediately-invoked function expressions `var foo = (function(){return function(){}})()` when you want to use the module pattern.

# Objects

- All functions are objects, and they have properties and methods.  
- A function designed to create new objects, is called an object constructor.
- A function defined as the property of an object, is called a method to the object.  

```javascript
console.log(obj.a);   // a
obj.say();            // something
```

**Literal**
```javascript
var obj = {
    a: "a",
    say: function(){
        console.log("something");
    }
}
```

**Function declaration as a constructor**
```javascript
function Foo(){
    this.a = "a";
    this.say = function(){
        console.log("something");
    }
}

var obj = new Foo();
```

**Function expression as a constructor**
```javascript
var Foo = function(){
    this.a = "a";
    this.say = function(){
        console.log("something");
    }
}

var obj = new Foo();
```

**Class**
```javascript
class Foo {
    constructor(){
        this.a = "a";
    }
    say(){
        console.log("something");
    }
}

var obj = new Foo();
```

**Prototype**
```javascript
function Foo(){};

Foo.prototype.a = "a";
Foo.prototype.say = function(){
    console.log("something");
}

var obj = new Foo();
```

**Singleton**
```javascript
var obj = new function(){
    this.a = "a";
    this.say = function(){
        console.log("something")
    }
}
```

# Looping
```javascript
Object.keys(myObj).forEach(key => {
    console.log(key);          // the name of the current key.
    console.log(myObj[key]);   // the value of the current key.
});
```














