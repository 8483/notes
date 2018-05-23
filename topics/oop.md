# this

The `object` that is executing the current function.

If a function is a `method`, i.e. belonging to an `object`, `this` references the `object`.

If a function is a regular one i.e. outside of an `object`, it references the `window`(browsers) or `global` (node) `object`.

99% of the problems occur with callbacks, because they are executed as regular functions i.e. not as methods, meaning `this` is not what it seems as the functions are executed by the window/global object.

```javascript
const video = {
    title: "Title",
    tags: ["a", "b", "c"],
    showTags: function(){
        this.tags.forEach(function(tag){ // Callback
            console.log(this.title + " " + tag); // This refers to the window object, not the video
        })
    }
}

video.showTags() // undefined a, undefined b, undefined c
```
We need to bind the anonymous callback to the corresponding object.

```javascript
const video = {
    title: "Title",
    tags: ["a", "b", "c"],
    showTags: function(){
        this.tags.forEach(function(tag){
            console.log(this.title + " " + tag);
        }.bind(this)) // The binding is done here
    }
}

video.showTags() // Title a, Title b, Title c
```

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

### Function parameters vs arguments
An argument is the value supplied to the parameter.
```javascript
function foo(bar){     // bar is a parameter
    console.log(bar);
}

foo("baz");            // baz is an argument.
```

# Objects

- All functions are objects, and they have properties and methods.  
- A function designed to create new objects, is called an object constructor.
- A function defined as the property of an object, is called a method to the object.  

**Literal**
```javascript
// Old way
var obj = {
    a: "a",
    say: function(){
        console.log("something");
    }
}

// New way
var obj = {
    a, // If the value key pair has the same name
    say(){
        console.log("something");
    }
}
```

**Factory**
```javascript
function createFoo(){ // camelCase
    return {
        a: "a",
        say(){
            console.log("something");
        }
    }
}

var obj = createFoo();
```

**Function declaration as a constructor**
```javascript
function Foo(){ // PascalCase
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

# Looping over properties
```javascript
Object.keys(myObj).forEach(key => {
    console.log(key);          // the name of the current key.
    console.log(myObj[key]);   // the value of the current key.
});
```









