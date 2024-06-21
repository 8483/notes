# Tips

-   Do not hold down motion keys, it's an anti-pattern.
-   Multi-cursors are an anti-pattern.

# Terminology

```bash
<C-a>     # CTRL + a
<CR>      # Enter
<tab>     # Tab
<esc>     # Escape
<space>   # Space
```

# Modes

```bash
ESC / CTRL + C    # Normal (commands)    █ Thick cursor
i                 # Insert (editing)     │ Thin cursor

v                 # Visual (selection) (not used often)
SHIFT + v         # Visual Line (select lines) (not used often)

CTRL + w          # Window (window selection)
CTRL + o          # Close all windows
```

# Normal mode

```bash
:           # Command
.           # Repeat previous command
u           # UNDO
CTRL + r    # REDO

zz          # Recenter screen (NEVER USE, it interferes with ZZ i.e. saving)
```

# Commands

```bash
:w     # Save
:q     # Quit
:wq    # Save & quit
:q!    # Force quit

:<command> <tab>  # Show available command options (CTRL + d also works)

:h <command>      # Manual
:set <option>     # Set an option

:tabe FILEPATH    # Open file in new tab
:tabn             # Next tab
:tabp             # Previous tab

:colorscheme      # Pick a colorscheme

:so               # Source i.e reload file
```

# File navigation

```bash
vim <path>    # Open directory inside vim and select a file.
vim <file>    # Open file directly.

:Ex           # Show current directory
:Vex          # Show directory in a new window
:e            # Show files in directory

:jumps              # Show navigation history
CTRL + o            # Navigate back
CTRL + i            # Navigate forward
CTRL + SHIFT + 6    # Switch between current and last opened file.
```

# Motions

All of these can be used in **visual (selection)** mode too.

```bash
j   # Down
k   # Up
h   # Left (not used often)
l   # Right (not used often)

w   # Forward one word.
b   # Back one word.
e   # End of word.
```

Adding a number before a command, will repeat it n times. Ex. `3h` will go up 3 lines, `5dd` will delete 5 lines.

`0` - Beginning of line.  
`$` - End of line.

`%` - Jump to matching tag i.e. `)`, `}`, `]`.

`f` + `character` - Jump **to** next character.  
`t` + `character` - Jump **before** next character.

`*` - Next occurrence of word under cursor.  
`#` - Previous occurrence of word under cursor.

`gg` - Beginning of file.  
`SHIFT + g` - End of file.  
`nG` - Jump to line n.

# Inside

`command` + `i` + `character` - Do a command inside characters.

`vi"` - Select everything inside "".  
`xi(` - Delete everything inside ().
`ci{` - Change (Delete and enter insert mode) inside {}.

# Selection

`esc` + `v` - visual mode for selection.  
`ctrl` + `v` - block selection i.e. multi-line column, good for commenting.  
`shift` + `i` then `text` then `esc` for multi line insertion.

# Copy & Paste

Enter **visual mode** and select text.

`y` - Copy (yank) selected.  
`d` - Cut selected.  
`p` - Paste after cursor/line.  
`P` - Paste before cursor/line.

`x` - Delete selected.

These can be combined with movements. Ex. `x2e` is delete next 2 words. `7dd` delete 7 lines.

`yy` - Copy (yank) line in **normal** mode.  
`dd` - Cut line in **normal** mode. (Shift + d)

:reg - Register i.e. list of yanks and deletions

# Search

`/` + `string` + `Enter` = Search for string **forwards**.  
`?` + `string` + `Enter` = Search for string **backwards**.  
`n` - Next occurrence.  
`N` - Previous occurrence.

# Replace

`:%s/text/replacement/g` - Replace text on every line.  
`:s/text/replacement/g` - Replace text on current line.

# Insert

`i` - Inserting with cursor on **inside**.  
`SHIFT + i` - Inserting with cursos at **begining** of line.

`a` - Inserting with cursor on **outside**.  
`SHIFT + a` - Inserting with cursos at **end** of line.

`o` - Create new line **under** cursor and enter **INS** mode.  
`O` - Create new line **above** cursor and enter **INS** mode.

`x` - Delete character.  
`X` - Backspace.

`r` + `new character` - Change character under cursor without INS mode.

# Multiple Inserts

`n times` + `i` + `string` + `ESC` = Multiple inserts.

Ex. `5ifoo` + `ESC` will result in `foofoofoofoofoo`.

# Remapping

**BE CAREFUL** not to remap important keys.

**Always have a `leader` key** i.e. A small pause that lets you execute a shortcut before the keys do their default behavior.

```bash
# Setup a show filetree shortcut
let mapleader = " "            # Sets <space> as a command activator
nnoremap <leader>tr :Vex<CR>   # Pressing space + tr executes :Vex + Enter (filetree)

n      # Mode where it works (normal mode)
nore   # No recursive execution i.e. prevent remaps activating other remaps
map    # Mapping left:right (replace left with right command)
```

# vimrc - Customization

Settings are not saved in vim. They need to be persisted in a `.vimrc` file.

The global `.vimrc` file is located in `/etc/vim/vimrc` or `etc/vimrc`.

You should create a local `.vimrc` file in the `home` directory for customizations.

When vim is opened, it will automatically check the current user's home directory for a `.vimrc` file. All settings specified in this file will override the global settings.

Configurations can be made inside `vim` by using `:set number`.

```bash
:set scrolloff=8      # Screen auto-scrolls to follow you, avoid recentering.
:set number           # Show line numbers.
:set relativenumber   # Line numbers are relative to the current line.

:set nocompatible     # Set compatibility to Vim only.
:set visualbell	      # Use visual bell (no beeping)
:set wrap             # Word wrap.

:set autoindent	      # Auto-indent new lines
:set shiftwidth=4     # Number of auto-indent spaces
:set smartindent	  # Enable smart-indent
:set smarttab	      # Enable smart-tabs
:set softtabstop=4    # TAB is 4 spaces.
:set expandtab        # Converts tabs into spaces.
:set tabstop=4        # Converts tabs into spaces.
```
