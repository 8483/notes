# General

Electron is a framework which allows to create desktop apps with HTML, CSS and Javascript. It's basically a Chrome browser with less features. It can also access the file system, whilst web apps cannot.

The HTML, CSS and Javascript can compile to executables for Windows, Mac or Linux.

Some notable apps that are built with it are Atom, VS Code and Slack.

**Try and keep in mind that Electron is simply a "manager" for a browser instance.**

**As soon as you're working in a renderer process, you're writing standard javascript and Electron really doesn't matter any longer.**

# Install

Don't install Electron as a global because then other people have to set stuff up to run your app (i.e. they now have to micromanage which version of Electron they have installed globally).

```bash
npm install electron --save-dev
```

**All tools should be installed as dev dependencies.**

### Errors

When running `npm install electron`, some users occasionally encounter installation errors.

In almost all cases, these errors are the result of network problems and not actual issues with the electron npm package. Errors like `ELIFECYCLE`, `EAI_AGAIN`, `ECONNRESET`, and `ETIMEDOUT` are all indications of such network problems.

**The best resolution is to try reconnecting, switching networks, or just waiting for a bit before trying to install again.**

# Run

**Before anything, make sure that `package.json` is updated with the relevant entry file. Default is `index.js`.**

```json
"main": "main.js"
```

**The electron library is not used in the final app build.**

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

Using the wrong one will result in an `ELIFECYCLE` error.

### ENOSPC error

Listen uses inotify by default on Linux to monitor directories for changes. It's not uncommon to encounter a system limit on the number of files you can monitor. For example, Ubuntu Lucid's (64bit) inotify limit is set to 8192.

When this limit is not enough to monitor all files inside a directory, the limit must be increased for Listen to work properly. Set a permanent limit with:

```bash
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```

# Simple app

**main.js**

```javascript
const electron = require("electron");
const { app, BrowserWindow } = electron;

let mainWindow;

function createWindow() {
    mainWindow = new BrowserWindow({ width: 800, height: 600 });
    mainWindow.loadURL(`file://${__dirname}/index.html`);
    mainWindow.on("closed", function() {
        mainWindow = null;
    });
}

app.on("ready", createWindow);
```

We keep the `mainWindow` variable in the global scope in order to prevent it from being garbage collected, thus losing the window.

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

# Main vs Renderer Process

Electron operates in two runtime contexts i.e. processes.

-   **Main** (one) / Node.js / Server / main.js
    -   File system access.
    -   Compiled module support.
    -   CommonJS modules.

*   **Renderer** (many) / Chromium browser / Client side
    -   HTML and CSS renderer.
    -   DOM access.
    -   Web API.

The main process can create many renderer processes (browser windows), but there is only one main process handling this.

# IPC (Inter Process Communication)

It allows us to send messages (with a payload) between the Main and Renderer processes. To use this, we need:

-   The `ipcMain` module in the `Main` process.
-   The `ipcRenderer` module in the `Renderer` processes.

By default, these modules send messages asynchronously, which is recommended.

## ipcMain

`ipcMain` can only listen for `Renderer` messages and reply to them. It's possible to send a message to a `Renderer` process by using `webContents.send()`.

```javascript
// Main
var ipcMain = require("electron").ipcMain;

win = new BrowserWindow();

ipcMain.on("asynchronous-message", (e, args) => {
    console.log(args); // prints "ping"
    event.sender.send("asynchronous-reply", "pong");
});

win.webContents.send("channel", "foo!");
```

## ipcRenderer

`ipcRenderer` can send messages to the `Main` and `Renderer` processes **and** listen for messages from them.

```javascript
// Renderer
var ipcRenderer = require("electron").ipcRenderer;

ipcRenderer.send("asynchronous-message", "ping");

ipcRenderer.on("asynchronous-reply", (e, args) => {
    console.log(args); // prints "pong"
});

ipcRenderer.on("channel", (e, args) => {
    console.log(args); // Prints 'foo!'
});
```

## Remote

Basically the `remote` module makes it easy to do stuff normally restricted to the main process in a render process without lots of manual ipc messages back and forth.

In Electron, GUI-related modules (such as dialog, menu etc.) are only available in the main process, not in the renderer process. In order to use them from the renderer process, the ipc module is necessary to send inter-process messages to the main process. **With the remote module, you can invoke methods of the main process object without explicitly sending inter-process messages**, similar to Java's RMI. An example of creating a browser window from a renderer process:

```javascript
const remote = require("electron").remote;
const BrowserWindow = remote.BrowserWindow;

var win = new BrowserWindow();
win.loadURL("https://github.com");
```

# Examples

## Developer Tools

```javascript
// Add this line in the createWindow main process.
mainWindow.webContents.openDevTools();
```

## Menu

If there's no custm menu specified, Electron shows a default one. Using a custom one overrides it.

```javascript
const electron = require("electron");
// We need to pull menu.
const { app, BrowserWindow, Menu } = electron;

let mainWindow;

function createWindow() {
    mainWindow = new BrowserWindow({ width: 800, height: 600 });
    mainWindow.loadURL(`file://${__dirname}/index.html`);

    // Build menu
    const mainMenu = Menu.buildFromTemplate(mainMenuTemplate);
    // Set Menu
    Menu.setApplicationMenu(mainMenu);
}

app.on("ready", createWindow);

// Array of menu objects
const mainMenuTemplate = [
    {
        label: "Menu item",
        submenu: [{ label: "Submenu item 1" }, { label: "Submenu item 2" }]
    }
];
```

## New Window

```javascript
const electron = require("electron");
// We need to pull menu.
const { app, BrowserWindow, Menu } = electron;

let mainWindow;

// Main window.
function createWindow() {
    mainWindow = new BrowserWindow({ width: 800, height: 600 });
    mainWindow.loadURL(`file://${__dirname}/index.html`);

    // Build menu
    const mainMenu = Menu.buildFromTemplate(mainMenuTemplate);
    // Set Menu
    Menu.setApplicationMenu(mainMenu);
}

// Create new window.
function createNewWindow() {
    newWindow = new BrowserWindow({
        width: 600,
        height: 400,
        title: "New Window"
    });
    newWindow.loadURL(`file://${__dirname}/newWindow.html`);
}

app.on("ready", createWindow);

// Array of menu objects
const mainMenuTemplate = [
    {
        label: "New window",
        click() {
            // Make the menu item clickable.
            createNewWindow(); // Call this in the main process.
        }
    }
];
```

## Shortcuts (Accelerators)

# Electron Reload

An npm library used for tracking changes in `.html` and `.js` rederer files, to avoid closing/starting electron to refresh.

```bash
npm install electron-reload --save-dev
```

```javascript
// main.js
require("electron-reload")(__dirname);
```

# Packaging

The app's general information is contained in `package.json`. We should always package the app from a copy, instead of the working file.

The dev-dependencies should also be removed before packaging, along with the code requiring them.

## Electron Packager

```bash
sudo npm install -g electron-packager
```

There are two ways of packaging the app.

```bash
# Electron as a dev-dependency, to avoid packaging the module as well.
electron-packager .

# Electron version as a flag. The version will be downloaded.
electron-packager . --electron-version="1.8.2"
```

### Platform

If we don't specify a platform, the app will be packaged in the host OS.

```bash
# Linux
electron-packager . --platform=linux --arch=x64

# Windows
electron-packager . --platform=win32 --arch=ia32
```

### asar

We can see the source files via `Contents/Resources/app`. We can hide these by packaging them in an `.asar` archive. This is similar to a `.tar` file, but without compression. This archive can be easily unpacked, but it does provide an extra layer of security.

```bash
# Use asar to hide the source code.
electron-packager . --asar=true
```

The source code would now be in `Contents/Resources/app.asar`.

### icon

The icon should be a square picture in several formats. `.png` for Linux, `.ico` for Windows, and `.icns` for Mac. A simple tool for these conversions is http://iconverticons.com. The icons need to be located in the source folder.

```bash
electron-packager . --icon=icon
```

### Flags

```bash
# Overwrites the previous packaging with the new one.
--overwrite
```

### Full commands

[Source](https://www.christianengvall.se/electron-packager-tutorial)

```bash
# Linux
electron-packager . electron-tutorial-app --overwrite --asar=true --platform=linux --arch=x64 --icon=assets/icons/png/1024x1024.png --prune=true --out=release-builds

# Windows
electron-packager . electron-tutorial-app --overwrite --asar=true --platform=win32 --arch=ia32 --icon=assets/icons/win/icon.ico --prune=true --out=release-builds --version-string.CompanyName=CE --version-string.FileDescription=CE --version-string.ProductName="Electron Tutorial App"
```

## Electron Builder

Compared to electron-packager, this has many more features and allows us to package, sign and release the app as a part of a highly configurable workflow. The main use is the ability to publish versions automatically.

```bash
sudo npm install -g electron-builder
```

In order to use this module for efficiently, we can add it as a dev-dependency and run the commands as npm scripts in package.json.

# Updating

## Electron Updater

Use `autoUpdater` from `electron-updater` instead of electon. It allows to listen for various update events after checking a URL.

```bash
npm install electron-updater
```

```javascript
import { autoUpdater } from "electron-updater";

autoUpdater.on("update-available", () => {});
```

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
