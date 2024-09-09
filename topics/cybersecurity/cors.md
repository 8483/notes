# Cross-origin resource sharing (CORS)

CORS is only required when a web page makes a request to a domain different from the one that served the web page.

If everything is on the same origin, the browser doesn't consider it a cross-origin request, and CORS is not needed.

---

[CORS is stupid](https://kevincox.ca/2024/08/24/cors/)

It prevents malicious injection in the frontend to have the backend send data where it shouldn't.

The reason CORS exists, is so you can't visit my cool website and execute my JS script which deletes your Facebook account on your behalf and also steals all your Gmail inbox data.

1. Log in to `https://your-bank.example.`
2. Visit `https://fun-games.example`.
3. `https://fun-games.example` runs `fetch("https://your-bank.example/profile")` to read sensitive information about you like your address and current balance.

This is where CORS comes in. It describes a policy for how cross-origin requests can be made and used. It is both incredibly flexible and completely insufficient.

The default policy allows making requests, but you can’t read the results. So fun-games.example is blocked from reading your address from `https://your-bank.example/profile`. It can also use side channels such as latency and if the request succeeded or failed to learn things.

But despite being incredibly annoying this doesn’t actually solve the problem! While fun-games.example can’t read the result, the request is still sent. This means that it can execute `POST https://your-bank.example/transfer?to=fungames&amount=1000000000` to transfer one billion dollars to their account.

This must be one of the biggest security compromises ever made in the name of backwards compatibility. The TL;DR is that the automatically provided cross-origin protections are completely broken. Every single site that uses cookies needs to explicitly handle it.

Yes, every single site.

---

CORS is a security feature implemented by web browsers to control how web pages can request resources from a different domain than the one that served the web page. This is crucial for preventing malicious websites from accessing sensitive data from another site.

CORS allows servers to specify who can access their resources and how they can be accessed. Without CORS, websites would be unable to interact with APIs or resources on different domains, severely limiting the functionality of modern web applications.

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

# How it works

When a web page makes a request to a different domain (a cross-origin request), the browser sends an HTTP request with an Origin header. The server then responds with specific CORS headers to indicate whether the request is allowed. Here are the main headers involved:

-   `Access-Control-Allow-Origin`: Specifies which origins are permitted to access the resource.
-   `Access-Control-Allow-Methods`: Indicates the HTTP methods allowed for the resource.
-   `Access-Control-Allow-Headers`: Lists the headers that can be used during the actual request.
