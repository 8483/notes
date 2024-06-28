# Starting VS Code in WSL (Ubuntu)

If developing with the Windows Subystem for Linux (WSL), it's a good idea to connect VS Code to it to **avoid errors** and get more functionalities.

This can be done by opening a directory from a WSL terminal with `code .` or by `CTRL` + `Shift` + `P` and choosing `WSL: Connect to WSL`.

# Shortcuts

```bash
# General

CTRL + p                       # Search files
CTRL + SHIFT + p               # Command palette
CTRL + ,                       # Settings

# Selection

ALT + click (empty)            # Multiple cursors

CTRL + d                       # Select next duplicate value
CTRL + u                       # Unselect duplicate value
CTRL + SHIFT + L               # Select all duplicate values
CTRL + k & CTRL + d            # Skip next duplicate value

CTRL + L                       # Select line. Press again selects next line

ALT + SHIFT + click            # Multiple selections in range
ALT + SHIFT + i                # Add cursor to each line in selection
ALT + SHIFT + left/right       # Select everything between brackets
ALT + SHIFT + left/right       # Select everything between brackets
CTRL + ALT + up/down           # Add cursor above/below

FN + left                      # Beginning of line
FN + right                     # End of line

# Jumping

CTRL + click (variable)        # Jump to definition
ALT + left/right               # Jumpt to previous/next editing location

# Utility

ALT + up/down                  # Move line up or down
ALT + SHIFT + up/down          # Duplicate line

CTRL + i                       # Show code completion suggestions
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
