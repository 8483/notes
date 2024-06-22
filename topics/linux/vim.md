# Tips

-   Do not hold down motion keys, it's an anti-pattern.
-   Multi-cursors are an anti-pattern.

# Terminology

```bash
<C-a>     # CTRL + a
<CR>      # Enter i.e. carret return
<tab>     # Tab
<esc>     # Escape
<space>   # Space
```

**NOTE:** Capital letters = SHIFT + letter

# Modes

```bash
ESC / <C-C>    # Normal (commands)    █ Thick cursor
i              # Insert (editing)     │ Thin cursor

v              # Visual (selection) (not used often)
V              # Visual Line (select lines) (not used often)
<C-v>          # Vertical visual mode
o              # Switch between start/end of selection to expand.

<C-w>          # Window (window selection)
<C-o>          # Close all windows
```

# Normal mode

```bash
:           # Command
.           # Repeat previous command
u           # UNDO
<C-r>    # REDO

zz          # Recenter screen (NEVER USE, it interferes with ZZ i.e. saving)
```

# Commands

```bash
:<command> <tab>  # Show available command options (<C-d> also works)

:h <command>      # Manual
:set <option>     # Set an option
:colorscheme      # Pick a colorscheme
```

Commands can be executed over selections with `!`.

```bash
:'<'> ! sort | uniq

# :        Command prompt
# '<'>     Represent the selection
# !        Tells that a command is to be run
# sort     Command for sorting
# |        Pipe
# uniq     Command for filtering
```

# Save + Exit

```bash
:w     # Save
:q     # Quit
:wq    # Save & quit
:q!    # Force quit
```

# Tabs

```bash
:tabe FILEPATH    # Open file in new tab
:tabn             # Next tab
:tabp             # Previous tab
```

# File navigation

```bash
vim <path>    # Open directory inside vim and select a file.
vim <file>    # Open file directly.

:Ex           # Show current directory
:Vex          # Show directory in a new window
:e            # Show files in directory

:jumps        # Show navigation history
<C-o>      # Navigate back
<C-i>      # Navigate forward
<C-^>      # Switch between current and last opened file.
```

# Motions

All of these can be used in **visual (selection)** mode too.

Motions are composable i.e. `5dd` and `d4j` will both delete 5 lines.

```bash
j    # Down
k    # Up
h    # Left (not used often)
l    # Right (not used often)

w    # Jump one word/delimitation.
W    # Jump to next whitespace
b    # Back one word.
e    # End of word.

_    # Jump to first non-whitespace character in line
0    # Beginning of line.
$    # End of line.

f + char    # Jump to first character. F is reversed.
t + char    # Jump to before first character. T is reversed.

;    # Next occurrence of character.
,    # Previous occurrence of character.

gg   # Beginning of file.
G    # End of file.
nG   # Jump to line n.

%    # Jump between matching tags i.e. ), }, ].
o    # Jump between start/end of selection to expand.

<C-d>   # Jump down by half page
<C-u>   # Jump up by half page
```

# Insert

```bash
i       # Inserting with cursor on inside.
I       # Inserting with cursos at begining of line.

a       # Inserting with cursor on outside i.e. append.
A       # Inserting with cursos at end of line.

o       # Create new line under cursor + insert mode.
O       # Create new line above cursor + insert mode.

x       # Delete character.
X       # Backspace.

r + char   # Change character under cursor without INS mode.

<C-a>   # Increment number.
<C-x>   # Decrement number.
```

# Selection

```bash
o       # Switch between start/end of selection.

viw     # Select whole word.
viW     # Select every character between whitespaces.

vi`     # Select everything inside ``.
xi(     # Delete everything inside ().
di[     # Cut everything inside [].
ci{     # Delete + insert mode inside {}.

va{     # Select outside {}.

ESC v   # visual mode for selection.
<C-v>   # block selection i.e. multi-line column, good for commenting.

I + text + esc # Multi line insertion.

'<      # Text representation for start of selection.
'>      # Text representation for end of selection.
```

# Copy & Paste

Enter **visual mode** and select text.

```bash
CTRL + SHIFT + p    # Paste from outside

y            # Copy (yank) selected.

d            # Cut selected.
D            # Cut to the right of cursor, same as d$

p            # Paste after cursor/line.
P            # Paste before cursor/line.

x            # Delete selecttion/charcater.

yy           # Copy (yank) line in normal mode.
dd           # Cut line in normal mode. (Shift + d)

cc           # Delete line + insert mode
C            # Delete after cursor + insert mode

s            # Delete single character + insert mode
S            # Delete whole line from indent + insert mode

dg           # Delete to end of file.

:reg         # Register i.e. list of yanks and deletions
```

# Search

Works with regex.

```bash
/ + string + Enter  # Search for string forwards.
? + string + Enter  # Search for string backwards.

n                   # Next occurrence.
N                   # Previous occurrence.

grep foo \**/*js    # Every single files that ends with `.js`
```

# Replace

```bash
:s/text/replacement        # Only first occurence.
:s/text/replacement/g      # Only on current line.
:%s/text/replacement/g     # Every line in file.

v + select + : + s/text    # Only in selected range.
```

# Quickfixlist

This is a list with your `grep` search results.

```bash
:copen   # Show current (first) result
:cnext   # Show next result. Remap to <C-j>.
:cprev   # Show previous result. Remap to <C-k>.
```

Perform an action/macro on all results.

```bash
:cdo    # Run
:wa     # Save changes
```

# Macro

Replay keystrokes.

```bash
q + char    # Start recording, ex. q + a
q           # Stop recording
@char       # Execute macro, ex. @a

:reg        # List of macros.
```

# Register

A key value store. Macros are stored here, can be edited.

```bash
# \ is for escaping.

\" + char          # Access register at char, ex. "b
\" + char + y      # Copy into register
\" + char + p      # Paste from register

\"_                # Void register i.e. dev/null
\"+                # System clipboard
```

# Source/Reload configuration

```bash
:so
```

There are also special directories that auto-source.

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

# Plugins

> You can write your own plugins with Lua or VimL.

### 1. Install `vim-plug` plugin manager

A community made [plugin manager](https://github.com/junegunn/vim-plug) that uses only github repos specifically made to work with vim.

```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

Needs a vim reload.

### 2. Add plugins

Example for [fzf - fuzzy finder](https://github.com/junegunn/fzf.vim) plugin.

Add to `~/.vimrc` file.

```bash
call plug#begin()

Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'

call plug#end()
```

### 3. Source/reload `vimrc`

```
:so
```

### 4. Install the plugins

```bash
:PlugInstall
```

### 5. Add shortcuts i.e. remaps

```bash
nnoremap <C-p> :GFiles<CR>        # <C-p> to start fuzzy finder
nnoremap <leader>pf :Files<CR>    # SPACE + pf for another way
```

# Themes

ayu, gruvbox, monokai, dracula...
