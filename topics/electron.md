# General

Electron is a framework which allows to create desktop apps with HTML, CSS and Javascript. It's basically a Chrome browser with less features. It can also access the file system, whilst web apps cannot.  

The HTML, CSS and Javascript can compile to executables for Windows, Mac or Linux.  

Some notable apps that are built with it are Atom, VS Code and Slack.  

# Install
Don't install Electron as a global because then other people have to set stuff up to run your app (i.e. they now have to micromanage which version of Electron they have installed globally).
```bash
npm install electron --save-dev
```
**All tools should be installed as dev dependencies.**

# Run
Before anything, make sure that `package.json` is updated with the relevant entry file. Default is `index.js`.  
```json
"main": "main.js"
```

The electron library is not used in the final app build.  

If electron was installed locally in `node_modules`.  
```bash
./node_modules/.bin/electron .
npm start # We use the above command inside package.json.
```

If electron was installed globally.
```bash
electron main.js
electron . # Same as above. Looks for main.js inside current directory.
npm start # We use the above command inside package.json.
```


# Electron Reload
An npm library used for tracking changes in `.html` and `.js` rederer files, to avoid closing/starting electron to refresh.
```bash
npm install electron-reload --save-dev
```
```javascript
// main.js
require("electron-reload")(__dirname);
```

# Main vs Renderer Process
Electron operates in two runtime contexts i.e. processes.
- **Main** (one) / Node.js / Server / main.js
- **Renderer** (many) / Chromium browser / Client side

The main process can create many renderer processes (browser windows), but there is only one main process handling this.  

# Simple app
**main.js**
```javascript
const electron = require("electron");
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;

let mainWindow;

function createWindow() {
    mainWindow = new BrowserWindow({width: 800, height: 600})
    mainWindow.loadURL(`file://${__dirname}/index.html`)
    mainWindow.webContents.openDevTools();
    mainWindow.on("closed", function() {
        mainWindow = null;
    })
}

app.on("ready", createWindow);
```

**index.html**
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Window</title>
  </head>
  <body>
    <p>Some content</p>
  </body>
  <script>
    // We can put our logic here, or offload it with require("./renderer.js")
  </script>
</html>
```

# IPC

## Remote



# Packaging




## Electron Packager



## Electron Builder



# Native Node Modules
**This mostly applies to C based libraries such as bcrypt and ffi.**  

Electron is a pre-built library, and it is often built against a different version of the V8 node binary than your system. Installing native npm packages normally would result in an error like this...  
```bash
App threw an error during load.
Error: Module version mismatch. Expected 50, got 46.
```
In order to use native modules, we need to manually specify the location of the electron headers. To simplify this, we can use a custom script `electron-npm`, which we can call instead of `npm install` for native modules.

```bash
# electron-npm script
export npm_config_target=1.7.10 # Electron's version. Find with ./node_modules/.bin/electron -v
export npm_config_arch=x64 # The architecture.
export npm_config_disturl=https://atom.io/download/atom-shell # Download headers.
export npm_config_runtime=electron # Tell node-pre-gyp we are building for Electron.
export npm_config_build_from_source=true # Tell node-pre-gyp to build module from source code.
npm install $1 # Replace with the first argument passed.
```
Now, we can install the npm packages with...
```bash
./electron-npm package # Instead of "npm install package"
```

