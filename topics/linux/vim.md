# Learning

If you want to use vim as you'd use VS Code, just go with VS Code.

Vim works a different way, for a reason, don't try to make it VS Code.

When you’re working don’t try speed all the time, slow down and see if there’s a way to accomplish what you want easier.

# Tips

-   Live in normal mode. Spend as little time as possible in insert mode.
-   Avoid using plugins as much as possible.
-   Avoid using multi-cursors, learn the built in ways.
-   Do not hold down motion keys, it's an anti-pattern.

# Version

```bash
:version
```

# Terminology

```bash
<C-a>     # CTRL + a
<CR>      # Enter i.e. carret return
<tab>     # Tab
<esc>     # Escape
<space>   # Space / Leader
```

**NOTE:** Capital letters = SHIFT + letter

# Modes

```bash
Esc / <C-c> / <C-[>    # Normal (commands)    █ Thick cursor

i              # Insert (editing)     │ Thin cursor

v              # Visual (selection) (not used often)
V              # Visual Line (select lines) (not used often)
<C-v>          # Visual block (vertical) mode
o              # Switch between start/end of selection to expand.
```

# Normal mode

```bash
:           # Command
.           # Repeat last command

u           # UNDO
<C-r>       # REDO

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
:wq    # Save and quit
:q!    # Force quit
```

# File navigation

```bash
vim <path>    # Open directory inside vim and select a file.
vim <file>    # Open file directly.

:e <path>     # Navigate to directory/file. Use TAB and CTRL + d to choose.
:find <path>  # Navigate to directory/file.

:e.           # Show directory
:E            # Show directory
:Ex           # Show directory

:Lex          # Show directory in a side window
:Vex          # Show directory in a new window

:jumps        # Show navigation history
<C-o>         # Navigate back
<C-i>         # Navigate forward
<C-^>         # Switch between current and last navigation.

<C-g>         # Show current file name
```

# Wildmenu

Command autocompletion.

```bash
:set wildmenu           # Turns the feature on.

:e string               # Search in current directory.
:e **/*string           # Search recursively from root.

<tab>                   # Show wildmenu. Go forward.
<s-tab>                 # Go backward.

<c-d>                   # Print ALL suggestions.

<Left> / <Right>        # Traverse options.
<Down> / <Up>           # Drill out/down directories.

:set wildoptions=pum    # Makes the menu vertical. Traversal is reversed.
:set wildignore+=**/node_modules/**,**/.git/** # Ignore directories
:set wildignorecase     # Case insensitive
```

Wldignore is only applied after the search.

# path

When using `**/*`, it means:

-   `**` - Look inside all directories from where I am.
-   `/*` - Look for all files (and directories) within the directories found by `**`.

# :e vs :find

`find` search the `'path'` in vim. Whereas :edit only takes the current working directory as the root.

`:edit` is restricted by default to the working directory: if you need to edit a file that is not under your working directory you will have to provide its absolute path or a path relative to the working directory. Also, you need to provide the necessary globs.

`:find` is superficially very similar to `:edit` but the (big) difference is that it finds files in the directories specified in the `path` option. `path` is what makes `:find` a lot more interesting than `:edit`.

With `set path=,,` you essentially get the same behavior as `:e foo`.

With `set path=**` you essentially get the same behavior as `:e **/foo` except you don't have to use any glob.

With `set path=.,**` you also get access to files in the same directory as the current file.

With `set path=.,**,/path/to/some/central/vendor/directory` you also get access to files from that directory… and so on.

# Motions

All of these can be used in **visual (selection)** mode too.

Motions are composable i.e. `5dd` and `d4j` will both delete 5 lines.

```bash
j       # Down
k       # Up
h       # Left (not used often)
l       # Right (not used often)

w       # Jump one word/delimitation.
W       # Jump to next whitespace
e       # End of word.
b       # Back one word.

_       # Jump to first non-whitespace character in line
^       # Beginning of line.
0       # Beginning of line.
$       # End of line.

f + char    # Jump to first character in line. F is reversed.
t + char    # Jump to before first character in line. T is reversed.
;           # Next occurrence of character.
,           # Previous occurrence of character.

gg      # Beginning of file.
G       # End of file.
nG      # Jump to line n.

%       # Jump between matching tags i.e. ), }, ].
o       # Jump between start/end of selection to expand.

<C-d>   # Jump down by half page
<C-u>   # Jump up by half page

}       # Jump to next block/paragraph
```

# Editing

```bash
i           # Inserting with cursor on inside i.e prepend.
I           # Inserting with cursor at begining of line.

a           # Inserting with cursor on outside i.e. append.
A           # Inserting with cursor at end of line.

c + motion  # Change value in selection ex. cw, c_, ci, ca...

C           # Delete after cursor + insert mode
cc          # Delete line + insert mode

o           # Create new line under cursor + insert mode.
O           # Create new line above cursor + insert mode.

x           # Delete character.
X           # Backspace.

s           # Delete single character + insert mode
S           # Delete whole line from indent + insert mode

r + char    # Change character under cursor without INS mode.

<C-a>       # Increment number.
<C-x>       # Decrement number.
```

# Selection

```bash
o       # Switch between start/end of selection.

*       # Search forward for word under cursor
gn      # Search forward for word under cursor

\#      # Search backward for word under cursor

viw     # Select whole word.
viW     # Select every character between whitespaces.

vi`     # Select everything inside ``.
xi(     # Delete everything inside ().
di[     # Cut everything inside [].
ci{     # Delete + insert mode inside {}.

va{     # Select outside {}.

Esc v   # visual mode for selection.
<C-v>   # block selection i.e. multi-line column, good for commenting.

I + text + esc # Multi line insertion.

'<      # Text representation for start of selection.
'>      # Text representation for end of selection.
```

# Copy and Paste

Enter **visual mode** and select text.

```bash
right click  # Paste from system clipboard (insert mode)

:set paste + <S-i> # Formatted paste

y            # Copy (yank) selected.

d            # Cut selected.
D            # Cut to the right of cursor, same as d$

p            # Paste after cursor/line.
P            # Paste before cursor/line.

x            # Delete selecttion/charcater.

yy           # Copy (yank) line in normal mode.
dd           # Cut line in normal mode. (Shift + d)

dg           # Delete to end of file.

:reg         # Register i.e. list of yanks and deletions
```

# Indentation

```bash
>>      # Add indentation.
<<      # Remove indentantion.

<C-t>   # Add indentation at start of line (insert mode).
<C-d>   # Remove indentation at start of line (insert mode).
```

# Autocomplete

```bash
# Insert mode

:h ins-completion       # Manual

<C-n>                   # Next suggestion
<C-p>                   # Previous suggestion

<C-x><C-n>              # Next suggestion
<C-x><C-p>              # Previous suggestion

<Cx><C-f>               # Complete path

<Cx><C-l>               # Complete line


# Omnicomplete - language specific

<Cx><C-o>               # Show suggestions

:set omnifunc=javascriptcomplete#CompleteJS
:set omnifunc=htmlcomplete#CompleteTags
:set omnifunc=csscomplete#CompleteCSS
```

# Search

Works with regex.

```bash
n    # Next occurrence of string under cursor (Works without searching)
N    # Previous occurrence of string under cursor Works without searching

/ + string + Enter  # Search for string forwards.
? + string + Enter  # Search for string backwards.

grep foo \**/*js    # Every single files that ends with `.js`
```

# Replace

```bash
:s/text/replacement        # Only first occurence.
:s/text/replacement/g      # Every occurence in line.
:%s/text/replacement/g     # Every occurence in file.

v + select + : + s/text    # Only in selected range.

&                          # Repeat replace for string under cursor. (n + &)
g&                         # Repeat replace for all
```

# Multiple selection

Tries to simulate `CTRL` + `d` (find duplicate) functionality.

```bash
# 1. Go to top of file.
gg

# 2. Search
/foo

# 3. Edit
i / a / c       # Edit however you want ex. "cgn" change first from top.

# 4. Jump to next occurence
n

# 5. Repeat last edit
.
```

Another approach

```bash
# 1. Change any occurence
i / a / c

# 2. Apply change to all other occurences
:g/foo/norm .

:g/foo/   # Repeat command on every line that has foo.
norm .    # Execute the normal mode command . (repeats the last change)
```

# grep

Search for text inside of currently open file, or specified files.

```bash
:grep              # External (terminal) search

:vimgrep           # Internal (vim) search, add to quickfix list
:vim               # Internal (vim) search, add to quickfix list

:vim foo           # Current file, first occurence.
:vim /foo/г        # Current file, every occurence.

:vim foo *         # Current directory
:vim foo **        # Current directory and subdirectories
:vim foo **/*      # Current directory and subdirectories
:vim foo **/*js    # Current directory and subdirectories only in js files

:vimgrep /foo/g ~/bar.js ~/baz.js # Every foo in these files
```

# Quickfix List

This is a list with your `vimgrep` search results.

```bash
:vim foo **       # Current directory and subdirectories

:copen            # Show current (first) result

# Navigate while inside of window

j            # Show next result.
k            # Show previous result.

# Navigate while outside of window

:cnext            # Show next result. Remap to <C-j>.
:cprev            # Show previous result. Remap to <C-k>.

# Previous lists. Up to 10 saved.

:colder           # Previous list
:cnewer           # Next list

# Perform an action/macro on the list

:cdo command      # Run on every match individually in the quickfix list.
:cfdo command     # Run on the entire file containing a match in the quickfix list.
:wa               # Save changes
```

**Example**

```bash
:vimgrep foo %           # Find foo in current file, put results in quickfix list
:copen                   # Open the quickfix list
:cfdo %s/foo/bar/g       # Apply search and replace to every list item
```

# Commenting

```bash
<C-v>        # Enter visial block mode
jk / }       # Go up/down or jump to end of block
I            # Insert at beginning of line
//           # Add commenting characters
ESC          # Apply to all lines
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
# \" is an escaped "

\"a       # Access register for a
\"b       # Access register for b

\"ay      # Copy into register a
\"by      # Copy into register b

\"ap      # Paste from register a
\"bp      # Paste from register b

\"+       # System clipboard
\"_       # Void register i.e. dev/null
```

# Windows

```bash
<C-w>           # Window commands/mode.
<C-o>           # Close all windows

<C-w> w         # Cycle through open windows.
<C-w> <C-w>:    # Cycle through open windows.

<C-w> s         # Split the current window horizontally.
<C-w> v         # Split the current window vertically.

<C-w> c         # Close the current window.
<C-w> o         # Close all other windows except the current one.

<C-w> h         # Move to the window on the left.
<C-w> j         # Move to the window below.
<C-w> k         # Move to the window above.
<C-w> l         # Move to the window on the right.

<C-w> q         # Quit the current window.

<C-w> =         # Make all windows equal size.
<C-w> _         # Maximize the current window height.
<C-w> |         # Maximize the current window width.

<C-w> +         # Increase the height of the current window.
<C-w> -         # Decrease the height of the current window.
<C-w> >         # Increase the width of the current window.
<C-w> <         # Decrease the width of the current window.
```

# Tabs

```bash
:tabe FILEPATH    # Open file in new tab

gt                # Next tab
gT                # Previous tab

:tabn             # Next tab
:tabp             # Previous tab
```

# Suspend

```bash
<C-z>          # Suspend vim i.e. show terminal
fg + Enter     # Open back vim

:sh            # Suspend vim i.e. show terminal
exit           # Open back vim
```

# Terminal

```bash
! command      # Run a command with vim in the background
:term          # Open terminal in new window
```

# Source/Reload configuration

```bash
:so %     # % refers to the current file
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

```vim
set scrolloff=8      " Screen auto-scrolls to follow you, avoid recentering.
set number           " Show line numbers.
set relativenumber   " Line numbers are relative to the current line.

set nocompatible     " Set compatibility to Vim only.
set visualbell	     " Use visual bell (no beeping)
set wrap             " Word wrap.

set autoindent	     " Auto-indent new lines
set shiftwidth=4     " Number of auto-indent spaces
set smartindent	     " Enable smart-indent
set smarttab	     " Enable smart-tabs
set softtabstop=4    " TAB is 4 spaces.
set expandtab        " Converts tabs into spaces.
set tabstop=4        " Converts tabs into spaces.

set t_u7=            " Fix where vim starts in replace mode.
```

**NOTE:**

-   Configurations can be made inside `vim` by using `:set number`.
-   Commenting in `.vimrc` files is done with a single `" ` character (no closing).

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
:so ~/.vimrc
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

### 6. Delete plugins

Remove plugin from `~/.vimrc`.

```bash
:PlugClean
```

# vimrc example

```vim
" SETTINGS ========================================================================

set nocompatible     " Set compatibility to Vim only.
set path+=**         " Find searches recursively in subdirectories.

set wildmenu         " Show command autocompletion suggestions.
set wildoptions=pum  " Vertical wildmenu
set wildignorecase   " Case insensitive
set wildignore+=**/node_modules/**,**/.git/**  " Ignore directories

set scrolloff=8      " Screen auto-scrolls to follow you, avoid recentering.
set number           " Show line numbers.
set relativenumber   " Line numbers are relative to the current line.
set hlsearch         " Highligt searches

set visualbell       " Use visual bell (no beeping)
set wrap             " Word wrap.

set autoindent       " Auto-indent new lines
set shiftwidth=4     " Number of auto-indent spaces
set smartindent      " Enable smart-indent
set smarttab         " Enable smart-tabs
set softtabstop=4    " TAB is 4 spaces.
set expandtab        " Converts tabs into spaces.
set tabstop=4        " Converts tabs into spaces.

set t_u7=            " Fix buy where vim starts in replace mode.

set completeopt+=menuone     " Always show autocompletion
set completeopt+=noselect    " Manual selection required

filetype plugin on   " Detect filetypes and load relevant plugins.

" set cursorcolumn     " Show vertical line to locate cursor.
" set cursorline       " Show horizontal line to locate cursor.

" PLUGINS ========================================================================

call plug#begin()

" Themes
Plug 'tomasiser/vim-code-dark'

" Interface
Plug 'vim-airline/vim-airline'
" Plug 'blueyed/vim-diminactive'

" Syntax hihglighting
Plug 'pangloss/vim-javascript'
Plug 'evanleck/vim-svelte'

" Functionality
" Plug 'lifepillar/vim-mucomplete'
" Plug 'mg979/vim-visual-multi'
" Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
" Plug 'junegunn/fzf.vim'

" Formatting
" Plug 'prettier/vim-prettier', { 'do': 'yarn install --frozen-lockfile --production' }

call plug#end()

" PLUGIN SETTINGS =======================================================================

" let g:mucomplete#enable_auto_at_startup = 1

colorscheme codedark

" REMAPS ========================================================================

" nnoremap oo o<ESC>
" nnoremap OO O<ESC>

nnoremap <C-j> :cnext<CR>
nnoremap <C-k> :cprev<CR>

noremap <Up> <nop>
noremap <Down> <nop>
noremap <Left> <nop>
noremap <Right> <nop>

" noremap <C-p> :Files<CR>
```
