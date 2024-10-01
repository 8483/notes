# Connect VS Code to WSL (Ubuntu)

If developing with the Windows Subystem for Linux (WSL), it's a good idea to connect VS Code to it to **avoid errors** and get more functionalities.

This can be done by opening a directory from a WSL terminal with `code .` or by `CTRL` + `Shift` + `P` and choosing `WSL: Connect to WSL`.

When VS Code connects to a remote WSL environment, it sets up a remote development workspace inside your WSL distribution.

-   Extension Host Setup: VS Code installs a small server (vscode-server) on the WSL machine. This server is responsible for running extensions and communicating with the VS Code client on Windows.

-   File System Access: It gives VS Code direct access to your WSL file system. You can open files and folders from within the WSL environment, and VS Code treats them as local files.

-   Terminal Integration: The VS Code terminal connects directly to WSL, so when you open a terminal, it's running in WSL, not Windows. This allows you to run Linux commands and interact with your Linux environment directly.

-   Language and Debugging Support: The extensions installed in the remote environment handle language features, IntelliSense, and debugging. For example, if you're using Node.js inside WSL, VS Code will run the Node.js debugger from within WSL as well.

-   Seamless Extension Support: When you install extensions, VS Code will prompt you to install the extension either in the WSL environment or locally, depending on where it's needed. Extensions that work with WSL are installed directly inside WSL.

-   Network and Proxy Configuration: VS Code manages network configurations for the remote connection, including handling SSH keys, proxies, and other connection details.

In essence, the WSL remote connection allows you to work within your Linux environment while benefiting from the GUI and features of the Windows version of VS Code.

# Shortcuts

```bash
# General

CTRL + p                    # Search files fuzzy finder
CTRL + SHIFT + p            # Command palette
CTRL + ,                    # Settings

CTRL + TAB                  # Cycle through tabs
CTRL + w                    # Close tab

# Search and replace

CTRL + f                    # Search in file. ENTER = show next

CTRL + SHIFT + f            # Find all occurences of selection in project
SHIFT + F12                 # Show all occurences of selection
F2                          # Bulk renaming across whole project

# Duplicates

CTRL + d                    # Select next duplicate value
CTRL + u                    # Unselect duplicate value
CTRL + SHIFT + l            # Select all duplicate values
CTRL + k & CTRL + d         # Skip next duplicate value

# Multiple cursors

ALT + click (empty)         # Multiple cursors
ALT + SHIFT + click         # Multiple selections in range
CTRL + ALT + up/down        # Add cursor above/below

ALT + SHIFT + i             # Add cursor to each line in selection
ALT + SHIFT + left/right    # Select everything between brackets
ALT + SHIFT + left/right    # Select everything between brackets

# Line

ALT + up/down               # Move line up or down
ALT + SHIFT + up/down       # Duplicate line

FN + left                   # Beginning of line
FN + right                  # End of line

CTRL + l                    # Select line. Press again selects next line
CTRL + g                    # Jump to line

# Jumping

CTRL + click (variable)     # Jump to definition
ALT + left/right            # Jumpt to previous/next editing location

CTRL + FN + home            # Jump to beginning of file
CTRL + FN + end             # Jump to end of file

CTRL + SHIFT + o            # Jump to symbols (variables, functions)

# Utility

CTRL + i                    # Show code completion suggestions
CTRL + SPACE                # Show code completion suggestions

# Terminal

CTRL + ~                    # Toggle terminal
```

# Settings

These are located in the `settings.json` file, opened with `CTRL + SHIFT + P + 'settings'`.

```json
{
    // "git.enabled": false,
    // "git.path": null,
    // "git.autofetch": false,
    "git.ignoredRepositories": ["/mnt/c/user/dev/sandbox"],
    "git.ignoreMissingGitWarning": true,

    "workbench.activityBar.location": "top",
    "workbench.tree.indent": 20,
    "workbench.tree.renderIndentGuides": "always",
    "workbench.startupEditor": "none",

    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.unicodeHighlight.ambiguousCharacters": false,
    "editor.wordWrap": "on",

    "prettier.tabWidth": 4,
    "prettier.printWidth": 300,

    "[json]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[typescript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[svelte]": {
        "editor.defaultFormatter": "svelte.svelte-vscode"
    },
    "svelte.enable-ts-plugin": true,

    "window.zoomLevel": -1,

    "javascript.updateImportsOnFileMove.enabled": "always",

    "extensions.ignoreRecommendations": true
}
```

# Extensions

-   Autorenametag
-   Simple Alignment
-   Markdown Preview Enhanced
-   Multiple cursor case preserve
-   es6-string-html
-   WSL
-   DotEnv
-   Auto Rename Tag

# Workspace

The workspace folder order is controlled by a `.code-workspace` file.

```
{
    "folders": [
        {"path": "folder1"},
        {"path": "folder2"},
        {"path": "folder3"},
    ]
}
```

# Zen Coding (emmet)

```html
table.table.small>thead>tr>th*4
```

results in

```html
<table class="table small">
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th></th>
            <th></th>
        </tr>
    </thead>
</table>
```
