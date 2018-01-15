# General

Electron is a framework which allows to create desktop apps with HTML, CSS and Javascript. It's basically a Chrome browser with less features. It can also access the file system, whilst web apps cannot.

The HTML, CSS and Javascript can compile to executables for Windows, Mac or Linux.  

Some notable apps that are built with it are Atom, VS Code and Slack.  

# Install

```bash
sudo npm install -g electron # Global install.
```
# Run

The electron library is not used in the final app build.  

If electron was installed globally, we use this.

```bash
electron main.js
electron . # Same as above. Looks for main.js inside current directory.
npm start # We use the above command inside package.json.
```

If electron was installed locally in `node_modules`, we use a different command.  

```bash
./node_modules/.bin/electron .
npm start # We use the above command inside package.json.
```

# Main vs Renderer Process

