# socket.io

With RESTful HTTP you have a stateless request/response system where the client sends request and server returns the response.

With webSockets you have a stateful (or potentially stateful) message passing system where messages can be sent either way and sending a message has a lower overhead than with a RESTful HTTP request/response.

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

client

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

server

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
