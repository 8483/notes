Node does not give access to the global scope. It has a modular scope i.e. all the variables are scoped to the file, unless exported.  
```javascript
var message = "hello";
console.log(global.message); // undefined
```

# Modules
Every file in Node is considered a module. Everything declared inside is scoped to the file i.e. they are private. In order to use the content of a module, we need to export it i.e. make it public.