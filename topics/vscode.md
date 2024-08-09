# Starting VS Code in WSL (Ubuntu)

If developing with the Windows Subystem for Linux (WSL), it's a good idea to connect VS Code to it to **avoid errors** and get more functionalities.

This can be done by opening a directory from a WSL terminal with `code .` or by `CTRL` + `Shift` + `P` and choosing `WSL: Connect to WSL`.

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
