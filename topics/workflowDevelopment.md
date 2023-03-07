# tmux

Use to start development quickly:

-   Start front-end dev server.
-   Start MySQL server.
-   Start back-end API server.

```
tmux new-session \; \
    send-keys 'cd /home/user/dev/project/app && npm run dev' C-m \; \
    split-window -h -p 50 \; \
    send-keys 'echo password | sudo -S service mysql start && cd /home/user/dev/project/server && nodemon server.js' C-m \; \
```

# localhost Proxy

During development, the front-end and back-end run on different ports. In order to avoid messy "manual/env" `localhost` URLs, use a proxy configuration in the front-end dev server.

```js
// Vite example
server: {
    proxy: {
        "/api": "http://localhost:9010",
        "/auth": "http://localhost:9010",
        "/socket.io": {
            target: "ws://localhost:9010",
            ws: true,
        },
    },
}
```

# VPN

When working with databases that aren't yours, you can connect to them by establishing a VPN connection and using the local IP address of the machine hosting the database, example `192.168.10.4,1434` where `1434` is the port.

# Build

The front-end is bundled in static files, meaning they no longer run on a port. This means we can simply upload them to the live server to be used like any regular file.
