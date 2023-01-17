# Language

The complete JavaScript implementation is made up of three distinct parts:

-   The Core (based on ECMAScript spec)
-   The Document Object Model (DOM)
-   The Browser Object Model (BOM)

# ECMAScript

ECMA-262 describes it like this:

```
ECMAScript can provide core scripting capabilities for a variety of host environments, and therefore the core scripting language is specified... apart from any particular host environment.
```

A Web browser is considered a host environment for ECMAScript, but it is not the only host environment. A list of other host environments listed here.

Apart from DOM and BOM, each browser has its own implementation of the ECMAScript interface.

# Document Object Model (DOM)

The Document Object Model (DOM) is an application programming interface (API) for HTML as well as XML.

The DOM maps out an entire page as a document composed of a hierarchy of nodes like a tree structure and using the DOMAPI nodes can be removed, added, and replaced.

### DOM level 1

Consisted of two modules: the DOM Core, which provided a way to map the structure of an XML-based document to allow for easy access to and manipulation of any part of a document, and the DOM HTML, which extended the DOM Core by adding HTML-specific objects and methods.

### DOM Level 2

Introduced several new modules of the DOM to deal with new types of interfaces:

-   DOM Views — describes interfaces to keep track of the various views of a document (that is, the document before CSS styling and the document after CSS styling)
-   DOM Events — describes interfaces for events
-   DOM Style — describes interfaces to deal with CSS-based styles
-   DOM Traversal and Range — describes interfaces to traverse and manipulate a document tree

### DOM Level 3

Further extends the DOM with the introduction of methods to load and save documents in a uniform way (contained in a new module called DOM Load and Save) as well as methods to validate a document (DOM Validation). In Level 3, the DOM Core is extended to support all of XML 1.0, including XML Infoset, XPath, and XML Base.

Note that the DOM is not JavaScript-specific, and indeed has been implemented in numerous other languages. For Web browsers, however, the DOM has been implemented using ECMAScript and now makes up a large part of the JavaScript language.

Other DOMs

-   Scalable Vector Graphics (SVG)
-   Mathematical Markup Language (MathML)
-   Synchronized Multimedia Integration Language (SMIL)

# Browser Object Model (BOM)

Browsers feature a Browser Object Model (BOM) that allows access and manipulation of the browser window. Using the BOM, developers can move the window, change text in the status bar, and perform other actions that do not directly relate to the page content.

Because no standards exist for the BOM, each browser has its own implementation.

# Event Loop

The Javascript runtime only knows of the `heap` and `call stack`.

The rest of the functionality, like async stuff, is provided in the form of `WebAPIs` by the browser/Node.

For async operations, the `callback` is offloaded into a separate `queue`, which is emptied by the `event loop` one by one as soon as the `stack` becomes empty.

Without the `event loop` the stack would be blocked during the whole duration of the async operaiton, basically freezing the app.

![TEA](../pics/js_event_loop.png)

Based on [Philip Roberts' talk](https://www.youtube.com/watch?v=8aGhZQkoFbQ).
