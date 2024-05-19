# WSL

If developing with the Windows Subystem for Linux, it's a good idea to connect VS Code to it to **avoid errors** and get more functionalities.

This can be done by opening a directory from a WSL terminal with `code .` or by `CTRL` + `Shift` + `P` and choosing `WSL: Connect to WSL`.

# Shortcuts

```bash
# General

ctrl + ,                    # Settings
ctrl + shift + p            # Commands palette
ctrl + p                    # Search files

# Selection

alt + click                 # Multiple cursors

ctrl + d                    # Select next duplicate value
ctrl + u                    # Unselect duplicate value
ctrl + shift + L            # Select all duplicate values

ctrl + L                    # Select line. Press again selects next line

alt + shift + click         # Multiple selections in range
alt + shit + i              # Add cursor to each line in selection
alt + shift + left/right    # Select everything between brackets

# Utility

alt + up/down               # Move line up or down
alt + shift + up/down       # Duplicate line

ctrl + i                    # Show code completion suggestions
```

# Indentation

Go to settings with: `CTRL` + `,` and set:

-   Workbench > Tree: indent = 25
-   Workbench > Tree: Render Indent Guides = always

# Prettier

1. Install the `prettier` extension.
2. install the `svelte.svelte-vscode` extension.
3. `CTRL + SHIFT + P` command `settings.json` open settings
4. Add this line.

    ```json
    "[svelte]": {
        "editor.defaultFormatter": "svelte.svelte-vscode"
    }
    ```

5. Create a local `.prettierrc` file in the project.
6. Add this in the file.

    ```json
    {
        "trailingComma": "es5",
        "svelteSortOrder": "scripts-markup-styles",
        "tabWidth": 4
    }
    ```

7. **Explicitly restart the language server** with this command `Svelte: Restart Language Server`. **IT WILL NOT WORK WITHOUT THIS**. This needs to be done after each change to the settings.

# Zen Coding

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
