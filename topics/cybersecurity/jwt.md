Storing JWTs in localStorage or sessionStorage in a browser can make them vulnerable to XSS attacks. Storing them in a secure, HttpOnly cookie is generally safer.

After generating the JWT, send it to the client in an HttpOnly cookie. This ensures that the token cannot be accessed via JavaScript (protecting against XSS attacks).

```js
const express = require("express");
const jwt = require("jsonwebtoken");
const app = express();

app.post("/login", (req, res) => {
    // Authenticate user (you'd typically check the credentials here)
    const userId = req.body.userId; // example user ID

    // Generate the JWT
    const token = jwt.sign({ userId }, "your_secret_key", { expiresIn: "1h" });

    // Set the JWT in an HttpOnly cookie
    res.cookie("token", token, {
        httpOnly: true, // Prevents access to the cookie via JavaScript
        secure: true, // Ensures the cookie is sent over HTTPS (set to true in production)
        sameSite: "Strict", // Helps protect against CSRF attacks
    });

    res.send("Logged in successfully");
});
```

When the client makes requests to your server, the JWT will be automatically sent in the Cookie header. You can access and verify this token on the server to authenticate the request.

```js
app.get("/protected-route", (req, res) => {
    const token = req.cookies.token;

    if (!token) {
        return res.status(401).send("Access denied. No token provided.");
    }

    try {
        const decoded = jwt.verify(token, "your_secret_key");
        req.user = decoded; // Now you have access to the user info in req.user
        res.send("Protected data");
    } catch (err) {
        res.status(400).send("Invalid token");
    }
});
```

To log out a user, simply clear the cookie on the server.

```js
app.post("/logout", (req, res) => {
    res.clearCookie("token");
    res.send("Logged out successfully");
});
```

If your front-end relies on the token to determine whether to display a login page or a content page, making an API request on app load to check the token is a common and effective approach.

API Request on App Load: On the initial load of your front-end application, make an API request to a backend endpoint (e.g., /me, /profile, or /auth/check) that checks the validity of the token and returns the user’s information if the token is valid.

```js
// Example using Fetch API
function checkAuthStatus() {
    return fetch("/auth/check", {
        method: "GET",
        credentials: "include", // Include cookies in the request
    }).then((response) => {
        if (!response.ok) {
            throw new Error("Not authenticated");
        }
        return response.json();
    });
}

// Call this function on app load
checkAuthStatus()
    .then((user) => {
        // Token is valid; show content page
        console.log("User is authenticated:", user);
        showContentPage(user);
    })
    .catch((error) => {
        // Token is invalid or not present; show login page
        console.log("User is not authenticated:", error);
        showLoginPage();
    });
```

credentials: 'include': This ensures that the cookie containing the JWT is sent with the request.
