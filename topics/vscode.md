# Starting VS Code in WSL (Ubuntu)

If developing with the Windows Subystem for Linux (WSL), it's a good idea to connect VS Code to it to **avoid errors** and get more functionalities.

This can be done by opening a directory from a WSL terminal with `code .` or by `CTRL` + `Shift` + `P` and choosing `WSL: Connect to WSL`.

# Shortcuts

```bash
# General

ctrl + ,                    # Settings
ctrl + shift + p            # Commands palette
ctrl + p                    # Search files

# Selection

alt + click (empty)         # Multiple cursors

ctrl + d                    # Select next duplicate value
ctrl + u                    # Unselect duplicate value
ctrl + shift + L            # Select all duplicate values

ctrl + L                    # Select line. Press again selects next line

alt + shift + click         # Multiple selections in range
alt + shit + i              # Add cursor to each line in selection
alt + shift + left/right    # Select everything between brackets

# Jumping

ctl + click (variable)      # Jump to definition
alt + left/right            # Jumpt to previous/next editing location

# Utility

alt + up/down               # Move line up or down
alt + shift + up/down       # Duplicate line

ctrl + i                    # Show code completion suggestions
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
