# Cross Site Scripting (XSS)

**An XSS vulnerability enables an attacker to inject JavaScript into a site.**

Handling special characters properly by preventing them from being parsed as code.

-   Input validation

    -   Limit allowed user inputs by blacklisting `>`, `<`, `"`, `<script>`...

-   Input transformation

    -   Encoding input into HTML character enitites:
        -   `<` into `&lt;`
        -   `>` into `&gt;`

-   Escaping
    -   Having an input of `" alert("hacked")` as the quote ends the string in code, and executes the `alert`.
        -   Use backlashes to escape the quotes `\" alert("hacked")`.

#### Example

Let's say there's a website with an `input` form that:

-   Generates a query string.
-   Displays that string into the DOM.

```js
function ready() {
    let query = new URL(window.location).searchParams.get("query");
    document.getElementById("query-input").value = query;
    document.getElementById("query-output").innerHTML = query;
}
```

```
http://example.com/search?query=something
```

Now, putting this inside the input form...

```html
<img src onerror="alert(document.cookie)" />
```

Would inject the "string" via `innerHTML` as an image, which due to the missing `src`, would run the javascript code, which could be code sending the hacked the cookie via email.

The danger is when hackers send disquised links to other users to click.

To avoid this, use `innerText` instead!
