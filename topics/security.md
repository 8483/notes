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

**CHROME DOES NOT ALLOW CORS UNDER ANY CIRCUMSTANCES, not even localhost**

To bypass that, start chrome with `--disable-web-security` by running this command in `cmd`

```
"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --disable-web-security --user-data-dir=~/chromeTemp
```

http://50linesofco.de/post/2017-03-06-cors-a-guided-tour

This policy makes sure that a website can't read the response from a request made to another website and is enforced by the browser.

Ex. If you are on `example.org` you would not want that website to make a request to your banking website and fetch your account balance and transactions.

The "origin" in this case is composed from

-   Protocol (e.g. `http`)
-   Host (e.g. `example.com`)
-   Port (e.g. `8000`)

So `http://example.org` and `http://www.example.org` and `https://example.org` are three different origins.

CSRF (Cross Site Request Forgery) is not mitigated by the Same-Origin Policy.

Note that despite the Same-Origin Policy being in effect, our example request from thirdparty.com was successfully carried out to good.com - we just could not access the results. **For CSRF we don't need the result**...

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
