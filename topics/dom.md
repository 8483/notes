Instead of using JQuery for simple DOM manipulation, we can use Vanilla JS to achieve the same. JQuery is simply an abstraction library.  

# Select
```javascript
// By ID
document.getElementById('id');

// By class
document.getElementsByClassName('class-name'); // Returns an array

// By tag
document.getElementsByTagName('li'); // Returns an array
```

### Query Selector
Can select any element with the same command, but it grabs **only the first one**. This is pretty much JQuery if we replace `document.querySelector` with `$`.

```javascript
document.querySelector('#id'); // ID
document.querySelector('.class-name'); // Class
document.querySelector('li'); // Tag

// Select specific element by Class
document.querySelector('.class-name:nth-child(2)'); // Just the 2nd one.

// Select all elements
document.querySelectorAll('.class-name');

// Select all elements by Class
document.querySelectorAll('.class-name:nth-child(odd)'); // Every other one. Returns array.

// Examples
document.querySelector("header .container");
document.querySelector("header h1");
```
### Display
```javascript
// Show the class.
element.className;

// Return an array of classes. The array can be modified with .add or .remove
element.classList;
```

### Parent/Child
```javascript
var element = document.getElementById('id');

// Parent
element.parentNode // Selects the parent element.
element.parentNode.parentNode // Ad infinitum

// Child
element.children // Returns an array.
element.firstElementChild
element.lastElementChild

// Bad child. These include blank elements in the returned array.
element.childNode
element.firstChild
element.lastChild

// Sibling
element.nextElementSibling // Not nextSibling.
element.previousElementSibling // Not previousSibling.
```

# Change
```javascript
var element = document.getElementById('id');

// Text
element.textContent = "New text";
element.innerText = "New text";

// HTML
element.innerHtml = "<div>Text</div>";

// CSS
element.style.borderBottom = "solid 1px red";
```

### Common Elements
`document` - Whole DOM.  
`document.domain` - The domain name.  
`document.URL` - The URL.  
`document.title` - The title.  
`document.doctype` - The document type.  
`document.head` - The head object.  
`document.body` - The body object.  
`document.forms` - Array of all forms.  
`document.links` - Array of all links.  
`document.images` - Array of all images.  

# Create / Remove
```javascript
// Element
var element = document.createElement("div");

// Class
element.className = "class-name";

// classList
element.classList.add("class-name");
element.classList.remove("class-name");

// ID
element.id = "id";

// Attribute
element.setAttribute("title", "Hello World!");

// Text Node
var text = document.createTextNode("Hello World!");

// Add text to div
element.appendChild(text);

// Remove the child element
element.removeChild(text);
```

# Events

### Old way

```html
<button onClick="buttonClick()">Click here</button>
```
```javascript
function buttonClick(){
    console.log("Button clicked.");
}
```

### Event Listener
```html
<button id="button">Click here</button>
```
```javascript
var button = document.getElementById("button");

button.addEventListener("click", buttonClick);

function buttonClick(){
    console.log("Button clicked.");
}
```
We can pass in the event and do all kinds of things with it, as it contains information such as the event type, mouse coordinates etc.
```javascript
function buttonClick(e){
    console.log(e); // Logs the event.
    console.log(e.target); // The clicked element.
    console.log(e.target.id); // Clicked element id.
    console.log(e.target.className); // Clicked element class.

    console.log(e.ctrlkey); // Is control pressed boolean. altkey, shiftkey...
}
```

### Bulk Event Listeners
```javascript
document.querySelectorAll('#myTable td')
.forEach(e => e.addEventListener("click", function() {
    // Here, `this` refers to the element the event was hooked on
    console.log("clicked")
}));
```
Thit creates a separate function for each cell; **instead, you could share one function without losing any functionality.**

```javascript
function clickHandler() {
    // Here, `this` refers to the element the event was hooked on
    console.log("clicked")
}

document.querySelectorAll('#myTable td')
.forEach(e => e.addEventListener("click", clickHandler));
```

**Example**
```javascript
var hover = document.querySelectorAll('.hover');

// The functions are cointained inside another one to prevent execution on load.
hover.forEach(e => e.addEventListener("mouseover", () => mouseOver(e)));
hover.forEach(e => e.addEventListener("mouseout", () => mouseOut(e)));

function mouseOver(e) {
    e.classList.remove("w3-black");
    e.classList.add("w3-teal");
}

function mouseOut(e) {
    e.classList.remove("w3-teal");
    e.classList.add("w3-black");
}
```

### Mouse
```javascript
element.addEventListener("click", runEvent);
```

`click`- On click.  
`dblclick`- On double click.  
`mousedown` - On mouse button press.  
`mouseup` - On mouse button release.  
`mouseenter` - On hover over the element (parent) itself.  
`mouseover` - On hover over the child elements.  
`mouseleave` - On blur out of the element (parent) itself.  
`mouseout` - On blur out of child elements.  
`mousemove` - Any mouse movement.  

### Input
```javascript
var itemInput = document.querySelector('input[type="text"]');
var form = document.querySelector('form');
var select = document.querySelector('select'); // A selector with options.  

itemInput.addEventListener("keydown", runEvent); // Input events.
select.addEventListener("change", runEvent); // Selector events. "input" works the same.  

function runEvent(e){
    console.log(e.target.value); // Logs out the input value. Or selector value.
}
```
`focus` - On input click event.  
`blur` - Click outside of input event.  
`copy, cut, paste` - Input action events.  
`input` - Any interaction with the input.  

### Submit
Submitting a form registers an event for a split second and it vanishes. In order to prevent that and make it persistent, we need to use the event method `preventDefault()` i.e. it stops the normal form submission to a file.  

```javascript
form.addEventListener("submit", runEvent);

function runEvent(e){
    e.preventDefault();
    console.log(e.target.value); // Logs out the input value. Or selector value.
}
```

### Keyboard
`keydown` - Any keyboard button press and release.  
`keyup` - Button release.  
`keypress` - Button press.  

### Custom Events

```javascript
function send(channel, payload){
  let event = new CustomEvent(channel, {
    detail: payload
  })
  dispatchEvent(event);
}

function on(channel){
  addEventListener(channel, (e) => {
    console.log(e.detail)
  }, false); // bubbles
}

on("foo"); // Register listener

send("foo", "bar"); // Catch event and print "bar"
```
