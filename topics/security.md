# XSS - Cross Site Scripting

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

# CSRF / XSRF - Cross Site Request Forgery

**A CSRF vulnerability enables an attacker to perform actions on a website via an authenticated user.**

In a CSRF attack, the attacker makes a request to a third party page in the background, for instance by sending a POST request to your bank website. If you have a valid session with your bank, any website can make a request in the background that will be carried out unless your bank uses counter measures against CSRF.

# CORS Same-origin policy

It's a security feature that prevents a malicious site from making unauthorized requests to another site on behalf of a user.

It allows servers to specify who (i.e. which origins) can access their resources.

> CORS allows servers to control which domains can make API calls to them. Without the appropriate CORS headers, browsers will block cross-domain API calls by default.

The "origin" in this case is composed from

-   Protocol (e.g. `http`)
-   Host (e.g. `example.com`)
-   Port (e.g. `8000`)

These are three different origins:

```
http://example.org
http://www.example.org
https://example.org
```

> CORS is not there to protect the server. It is there to protect the client from cookie stealing and other client side attacks.

By default, web browsers prohibit web pages from making AJAX requests to a different domain than the one that served the web page. This is to prevent malicious sites from reading sensitive data from another site. CORS was introduced to relax this strict same-origin policy, not to enforce it.

When an AJAX request is made to a different domain, the browser checks for a CORS header in the response. If the server includes the appropriate CORS headers in its response (like Access-Control-Allow-Origin), then the browser permits the AJAX request, even across domains. If the headers are not present or do not match the requesting site's origin, then the browser blocks the request.

If you use a website and you fill out a form to submit information (your social security number for example) you want to be sure that the information is being sent to the site you think it's being sent to. So browsers were built to say, by default, "Do not send information to a domain other than the domain being visited".

> **CHROME DOES NOT ALLOW CORS UNDER ANY CIRCUMSTANCES, not even localhost**

To bypass that, start chrome with `--disable-web-security` by running this command in `cmd`

```
"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --disable-web-security --user-data-dir=~/chromeTemp
```

---

Why do direct API requests work in the browser?

When you access an API route directly in the browser, you are typically performing a simple HTTP GET request and viewing the results. In this scenario, the browser's same-origin policy isn't an obstacle because you're not trying to read the response programmatically from a different origin. You're simply navigating to a URL and viewing the content, just like you would when visiting any other website.

However, when you're working with a client-side app (like a JavaScript app running in a browser), things change. If the app tries to make an AJAX (or fetch, or XMLHttpRequest, etc.) request to an API on a different domain (a cross-origin request), the browser enforces the same-origin policy. The policy restricts web pages from making requests to a different domain than the one that served the web page.

To get around this, the server must include the appropriate CORS headers (like Access-Control-Allow-Origin) in its response to indicate that the client-side app from a different origin is allowed to read the response.

---

Prevents scripts from one origin to access private data on another origin.

CORS, or Cross Origin Resource Sharing, is a way to restrict what origins are allowed to access resources on your server. Say you have an API at someapi.com, and you hit it from somewebsite.com. You can set the CORS config at someapi.com to allow any origin, or you can configure it to only allow requests from somewebsite.com if you don't want any other website to be able to make API requests.

It's used to provide extra security and to protect against DDoS attacks.

CORS prevents one website domain from being able to request data from another website domain. Example: A phishing site called FooBar.com can't access your secure information from Megaglobalbank.com using malicious Javascript because CORS recognizes they are separate domains/origins.

# SQL Injection

Password:

```
password
```

Result:

```sql
SELECT *
FROM users
WHERE
    email = 'user@email.com'
    AND pass  = 'password' LIMIT 1
```

---

Use this to check for vulnerability.

Password:

```
'
```

Result:

```sql
SELECT *
FROM users
WHERE
    email = 'user@email.com'
    -- ERROR
    AND pass  = ''' LIMIT 1
```

---

This returns a session / JWT token

Password:

```sql
' or 1=1 --
```

Result:

```sql
SELECT *
FROM users
WHERE
    email = 'user@email.com'
    AND pass  = '' or 1=1 --' LIMIT 1

```

# MITM

Packet snooping.

# Good Practices

-   **SSL** - Let's encrypt.
-   **Encryption** - Password hashing.
-   **JWT** - Tokens vs cookies.
-   **Reverse-proxy** - Localhost vs direct.
-   **User groups** - Linux permissions.
