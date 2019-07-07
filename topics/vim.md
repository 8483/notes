# Vim

If a file is read-only and can't be changed, use `sudo vim file` to open it as root.

## Modes

`ESC` or `CTRL` + `[` - Command (Default)  
`i` - Insert (Editing)  
`v` - Visual (Like Command, but with selection)

## General

`:` - Commands.  
`.` - Repeat previous command.  
`u` - UNDO.  
`CTRL` + `r` - REDO.  
`c` + `movement` - Change up to movement.

# Inside

`command` + `i` + `character` - Do a command inside characters.

`vi"` - Select everything inside "".  
`xi(` - Delete everything inside ().
`ci{` - Change (Delete and enter insert mode) inside {}.

## Tabs

`:tabe FILEPATH` - Open file in new tab  
`:tabn` - Next tab  
`:tabp` - Previous tab

## Copy & Paste

Enter **visual mode** and select text.

`y` - Copy (yank) selected.  
`d` - Cut selected.  
`p` - Paste after cursor.

`x` - Delete selected.

These can be combined with movements. Ex. `x2e` is delete next 2 words.

`yy` - Copy (yank) line in **normal** mode.  
`dd` - Cut line in **normal** mode.

## Save & Exit

`:w` - Save  
`:q` - Quit  
`:wq` - Save & Quit  
`:q!` - Cancel & Quit

## Moving

All of these can be used in **visual** mode for selection.

`j` - Down  
`k` - Up  
`h` - Left  
`l` - Right

`w` - Forward one word.  
`b` - Back one word.  
`e` - End of word.

Adding a number before a command, will repeat it n times. Ex. `3h` will go up 3 lines, `5dd` will detele 5 lines.

`0` - Beginning of line.  
`$` - End of line.

`%` - Jump to matching tag i.e. `)`, `}`, `]`.

`f` + `character` - Jump **to** next character.  
`t` + `character` - Jump **before** next character.

`*` - Next occurrence of word under cursor.  
`#` - Previous occurrence of word under cursor.

`gg` - Beginning of file.  
`G` - End of file.  
`nG` - Jump to line n.

## Search

`/` + `string` + `Enter` = Search for string **forwards**.  
`?` + `string` + `Enter` = Search for string **backwards**.  
`n` - Next occurrence.  
`N` - Previous occurrence.

## Replace

`:%s/text/replacement/g` - Replace text on every line.  
`:s/text/replacement/g` - Replace text on current line.

## Insert

`o` - Create new line **under** cursor and enter **INS** mode.  
`O` - Create new line **above** cursor and enter **INS** mode.

`x` - Delete character.  
`X` - Backspace.

`r` + `new character` - Change character under cursor without INS mode.

## Multiple Inserts

`n times` + `i` + `string` + `ESC` = Multiple inserts.

Ex. `5ifoo` + `ESC` will result in `foofoofoofoofoo`.

# vimrc - Customization

The global `.vimrc` file is located in `/etc/vim/vimrc` or `etc/vimrc`.

Create a `.vimrc` file in the `home` directory for customizations.

When vim is opened, it will automatically check the current user’s home directory for a .vimrc file. All settings specified in this file will override the global settings.  

Configurations can be made inside `vim` by using `:set number`.

```bash
set nocompatible    # Set compatibility to Vim only.
set number          # Show line numbers.
set visualbell	    # Use visual bell (no beeping)
set wrap            # Word wrap.

set autoindent	    # Auto-indent new lines
set shiftwidth=4    # Number of auto-indent spaces
set smartindent	    # Enable smart-indent
set smarttab	    # Enable smart-tabs
set softtabstop=4   # TAB is 4 spaces.
```