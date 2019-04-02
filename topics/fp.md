# Functional Programming

Simple **is not** easy. Simple software is beautiful, and worth the effort.

For `objects`, we add properties with the `...spread` operator, and remove them with `...spread destructuring`.

For arrays, we add items also with the `...spread` operator, transform the array with `.map`, and remove items with `.filter`.

In both objects and arrays, we summarize info with `.reduce`.

# virtual-dom

The view part won't work with HTML strings. The elements have to be proper DOM nodes. Using strings gives this error `Uncaught TypeError: Failed to execute 'appendChild' on 'Node': parameter 1 is not of type 'Node'.`

```javascript
// Won't work
var el = "<p>test satu dua tiga</p>"; // is a string
document.body.appendChild(el);

// Works
var el = document.createElement("p"); // is a node
el.innerHTML = "test satu dua tiga";
document.body.appendChild(el);
```

# hyperscript-helpers

This small library helps make the code cleaner.

```javascript
// instead of writing
h("div");

// write
div();

// instead of writing
h("section#main", mainContents);

// write
section("#main", mainContents);
```

### API

```javascript
tagName(selector);
tagName(attrs);
tagName(children);
tagName(attrs, children);
tagName(selector, children);
tagName(selector, attrs, children);
Where;
```

-   `selector` is string, starting with "." or "#".
-   `attrs` is an object of attributes.
-   `children` is a hyperscript node, an array of hyperscript nodes, a string or an array of strings.

# Simple app

1. When we click the increment button, the `dispatch` function, defined in the `app` function is called, with the `MSGS.ADD` payload.
2. The `update` function, a switch statement, then uses the payload to match the behavior, in this case to add 1 to the current `model`.
3. The `view` function then uses the latest model to generate the DOM tree, but only with the changes parts, thanks to the `virtual-dom`.

```javascript
const hh = require("hyperscript-helpers"); // Create HTML element functions
const { h, diff, patch } = require("virtual-dom");
const createElement = require("virtual-dom/create-element");

// const { div, button } = require('hyperscript-helpers')(h);
const { div, button } = hh(h);

const initModel = 0;

const MSGS = {
    ADD: "ADD",
    SUBTRACT: "SUBTRACT"
};

// Updates the model after a view action.
function update(msg, model) {
    switch (msg) {
        case MSGS.ADD:
            return model + 1;
        case MSGS.SUBTRACT:
            return model - 1;
        default:
            return model;
    }
}

// The dispatch reloads the view. Defined in app.
function view(dispatch, model) {
    return div([
        div({ className: "counter" }, `Count: ${model}`),
        button(
            {
                className: "button",
                onclick: () => dispatch(MSGS.ADD)
            },
            "+"
        ),
        button(
            {
                className: "button",
                onclick: () => dispatch(MSGS.SUBTRACT)
            },
            "-"
        )
    ]);
}

// impure code contained inside app function
function app(initModel, update, view, node) {
    // Initial view
    let model = initModel;
    let currentView = view(dispatch, model);
    let rootNode = createElement(currentView);
    node.appendChild(rootNode);

    function dispatch(msg) {
        model = update(msg, model);
        const updatedView = view(dispatch, model);
        const patches = diff(currentView, updatedView);
        rootNode = patch(rootNode, patches);
        currentView = updatedView;
    }
}

// Load app div
const rootNode = document.getElementById("app");

app(initModel, update, view, rootNode);
```

# HTTP effects

HTTP requests are triggered in a similar way to how the app performs state updates.

Normal updates:

-   view generates a message.
-   update is called with the new message and current model.
-   update returns the new model.
-   view uses the new model to re-render the view.

HTTP updates:

-   view generates a message.
-   update is called with the new message and current model.
-   update returns the new model, a url, and a message generating function.
-   the app function makes a server request with the url.
-   when the server responds, the message generating function returned from update, creates a new message that includes the server data.
-   update is called with the newly generated message.
-   update returns the new model.
-   view uses the new model to re-render the view.
