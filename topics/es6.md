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
It is not completely immutable. It only prevents the reassignment of a value to a constant, but it doesn't care about modifying the object.
```javascript
const pi = 3.14;
pi = 42; // TypeError: Assignment to constant variable.

const person = {
  name: "John"
}
person.name = "Mike";
console.log(person.name) // Mike. Doesn't prevent mutation.
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
This is a collection of unique values. If we pass in an array with duplicates, we will get a collection (**object**) with unique values back. A `weakSet` is a collection of **unique objects** only.
```javascript
var array = [1, 2, 3, 3];

var set = new Set(array);      // {1, 2, 3}

// Set methods
set.size;         // 3
set.add("cat");   // [1, 2, 3, "cat"]
set.delete(2);    // [1, 3, "cat"]

// Convert to array
var array = Array.from(set);   // [1, 2, 3]
var array2 = [...set];         // [1, 2, 3]
```

# Arrow Functions
```javascript
function hello(name){
    return "Hello" + name;
}
console.log(hello("Jack")); // Hello Jack

// As long as no curly braces are used after the fat arrow, the return is implicit.
var hello = (name) => "Hello " + name;
console.log(hello("Jack")); // Hello Jack
```

It provides a shorter syntax, and more importantly, it binds `this` lexically, avoiding the usage of `that` or `self`. `Arguments` and `this` inside arrow functions reference their outer function.  

Until arrow functions, every new function defined its own `this` value. An arrow function **does not** have its own `this`; the `this` value of the enclosing execution context is used.  

```javascript
const obj = {
  data: "John",
  getData() {
      console.log(this.data);
  },
  getData2: () => {
    console.log(this.data);
  }
};

obj.getData();  // John
obj.getData2();   // undefined
```

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

// Default values. They work only if a property is undefined. null, false and 0 are all still values!
var obj = {a: 1}

var {a, b = "something"} = obj
console.log(a) // 1
console.log(b) // something
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

# Spread
Used for copying or deleting properties from one object to another.

### Copy
```javascript
// Without spread
const meal = {
    id: 1,
    description: "Breakfast",
}

const updatedMeal = {
    id: meal.id,
    description: meal.description,
    calories: 600,
}

// With spread
const meal = {
    id: 1,
    description: "Breakfast",
}

const updatedMeal = {
    ...meal, // Injects the id and description properties from meal.
    description: "Brunch", // This has precedence over the spread operator.
    calories: 600,
}
```

### Delete
```javascript
const meal = {
    id: 1,
    description: "Breakfast",
    calories: 600
}

const = { id, ...mealWithoutId} = meal; // Destructuring.
console.log(mealWithoutId); // Object without the id property.
```

### Array
```javascript
const meals = [
    { id: 1, description: "Breakfast", calories: 420 },
    { id: 2, description: "Lunch", calories: 520 }
]

const meal = {
    id: 3,
    description: "Snack",
    calories: 180
}

const updatedMeals = [...meals, meal]; // Returns a new array with the addition.
```




















