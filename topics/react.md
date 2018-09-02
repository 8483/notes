React is a library that only takes care of rendering the view while making sure it's in sync with the state.

# React Create App

Zero-configuration setup.

```bash
sudo npm i -g create-react-app
create-react-app app-name
```

It will install:

-   Lightweight development server
-   Webpack (bundler)
-   Babel (transpiler)

It also enables hot module reloading, which means we don't have to re-build and refresh after each change.

### Errors

You appear to be offline. Falling back to the local Yarn cache.

```bash
sudo apt-get remove yarn && sudo apt-get purge yarn

npm config set registry "https://registry.npmjs.org"
```

### Eject

This will permanently add all the abstracted dependencies in `package.json` and the config files will appear. This allows detailed configurations.

```bash
npm run eject
```

# Hello World

```javascript
// React object from module.
import React from "react";
import ReactDOM from "react-dom";

const element = <h1>Hello World</h1>; // JSX

ReactDOM.render(element, document.getElementById("app"));
```

# JSX

It's syntactic sugar for creating HTML elements with javascript, instead of using templating engines.

```jsx
const element = <h1>Hello World</h1>;
```

Babel transpiles it into...

```javascript
var element = React.createElement("h1", null, "Hello World");
```

Since the compiled JSX makes a call to `React.createElement`, we need to import `React`, even if we don't use the object directly.

This returns an `Object`, a React element, which is part of the virtual-DOM.

# Virtual-DOM

Lightweight in-memory representation of the UI. Instead of re-building the DOM on each change, the virtual DOM sees the changes and it just updates the changed parts in the real DOM.

Whenever the state of the React element object changes...

-   React gets a new React element.
-   Compares it to the previous one to see the changes.
-   Update just the changed parts in the real DOM.

The DOM updating is done by the `render()` method in the `react-dom` module. It takes the object returned from JSX and renders the HTML inside the referenced div.

# Components

A piece of UI. A javascript class that has some state to be displayed and a render method.
