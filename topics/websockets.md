`WebSocket` = low level building block.  
`socket.io` = A library solving common use cases.

Used to build real-time features by establishing a two way connection (full-duplex, like phones) between a client and a server.

It's used to broadcast data, rather than constantly refreshing the page, or having a data pooling interval.

With RESTful HTTP you have a stateless request/response system where the client sends request and server returns the response.

With webSockets you have a stateful (or potentially stateful) message passing system where messages can be sent either way, and sending a message has a lower overhead than with a RESTful HTTP request/response.

# How it works

1. Client sends an HTTP request asking the server to open a connection.
2. Server responds with a `101 - switching protocols` response i.e. the handshake is complete and the TCP/IP connection is left open, allowing bi-directional messages with very low latency.
3. The connection closes when one of the parties leaves.

**All clients have a built in `WebSocket` class.**

# WebSockets

```
npm install ws
```

**client**

```js
const socket = new WebSocket("ws://localhost:8080");

// Listen for messages
socket.onmessage = ({ data }) => {
    console.log("Message from server ", data);
};

document.querySelector("button").onclick = () => {
    socket.send("hello");
};
```

**server**

```js
const WebSocket = require("ws");
const server = new WebSocket.Server({ port: "8080" });

server.on("connection", (socket) => {
    socket.on("message", (message) => {
        socket.send(`Roger that! ${message}`);
    });
});
```

# socket.io

It's both a client and a server library.

**client**

```
npm install socket.io-client
```

```js
const socket = io("ws://localhost:8080");

socket.on("message", (text) => {
    const el = document.createElement("li");
    el.innerHTML = text;
    document.querySelector("ul").appendChild(el);
});

document.querySelector("button").onclick = () => {
    const text = document.querySelector("input").value;
    socket.emit("message", text);
};
```

**server**

```
npm install socket.io
```

```js
const http = require("http").createServer();

const io = require("socket.io")(http, {
    cors: { origin: "*" },
});

io.on("connection", (socket) => {
    console.log("a user connected");

    socket.on("message", (message) => {
        console.log(message);
        io.emit("message", `${socket.id.substr(0, 2)} said ${message}`);
    });
});

http.listen(8080, () => console.log("listening on http://localhost:8080"));
```

# Example 2

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Chat App</title>
        <script defer src="http://localhost:3000/socket.io/socket.io.js"></script>
        <script defer src="script.js"></script>
    </head>
    <body>
        <div id="message-container"></div>
        <form id="send-container">
            <input type="text" id="message-input" />
            <button type="submit" id="send-button">Send</button>
        </form>
    </body>
</html>
```

**client**

```js
const socket = io("http://localhost:3000");

const messageContainer = document.getElementById("message-container");
const messageForm = document.getElementById("send-container");
const messageInput = document.getElementById("message-input");

const name = prompt("What is your name?");
appendMessage("You joined");
socket.emit("new-user", name);

socket.on("get-message", (data) => {
    appendMessage(`${data.name}: ${data.message}`);
});

socket.on("user-connected", (name) => {
    appendMessage(`${name} connected`);
});

socket.on("user-disconnected", (name) => {
    appendMessage(`${name} disconnected`);
});

messageForm.addEventListener("submit", (e) => {
    e.preventDefault();
    const message = messageInput.value;
    appendMessage(`You: ${message}`);
    socket.emit("send-message", message);
    messageInput.value = "";
});

function appendMessage(message) {
    const messageElement = document.createElement("div");
    messageElement.innerText = message;
    messageContainer.append(messageElement);
}
```

**server**

```js
const io = require("socket.io")(3000);

const users = {};

io.on("connection", (socket) => {
    socket.on("new-user", (name) => {
        users[socket.id] = name;
        socket.broadcast.emit("user-connected", name);
    });

    socket.on("send-message", (message) => {
        socket.broadcast.emit("get-message", { message: message, name: users[socket.id] });
    });

    socket.on("disconnect", () => {
        socket.broadcast.emit("user-disconnected", users[socket.id]);
        delete users[socket.id];
    });
});
```

# Example 3

**client**

```js
// Connect to the serve (Here you want to change the port if you changed it in the server)
const socket = io("localhost:80");

// On new vote update the chart
socket.on("update", (candidates) => {
    // Convert the candidates object into an array
    console.log(candidates);
});

// Make a new vote (Remember this is not a safe way to do this)
function vote(index) {
    socket.emit("vote", index);
}
```

**server**

```js
// Import dependencies
const express = require("express");
const app = express();

// If you change this remember to change it on the client side as well
const port = 80;

// Host the front end
app.use(express.static("client"));

// Start the server and initialize socket.io
const server = app.listen(port, () => console.log(`Listening at http://localhost:${port}`));
const io = require("socket.io")(server);

// Initialize candidates
const candidates = {
    0: { votes: 0, label: "JavaScript", color: randomRGB() },
    1: { votes: 0, label: "C#", color: randomRGB() },
    2: { votes: 0, label: "PHP", color: randomRGB() },
    3: { votes: 0, label: "Python", color: randomRGB() },
    4: { votes: 0, label: "Go", color: randomRGB() },
};

// On new client connection
io.on("connection", (socket) => {
    io.emit("update", candidates);

    // On new vote
    socket.on("vote", (index) => {
        // Increase the vote at index
        if (candidates[index]) {
            candidates[index].votes += 1;
        }

        // Show the candidates in the console for testing
        console.log(candidates);

        // Tell everybody else about the new vote
        io.emit("update", candidates);
    });
});

// Generate a random RGB color
function randomRGB() {
    const r = () => (Math.random() * 256) >> 0;
    return `rgb(${r()}, ${r()}, ${r()})`;
}
```
