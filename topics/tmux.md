# tmux

## Basic
`tmux` - Start tmux.  
`CTRL + B` - Command mode.

## Panes
`%` - Split horizontally (left/right).   
`"` - Split vertically (top/bottom).  
`arrows` - Change pane. Resize if holding command.  
`x` - Close pane.  

## Windows
`c` - New window.  
`numbers` - Change window.  
`,` - Name window.  
`w` - List windows.  
`.` - Move window. Asks for a number.  
`&` - Close window.

## Misc
`[` - Scrolling mode with arrows or PageUp/PageDown. Exit with `CTRL + C`.

# Configuration
In order to configurate tmux, a `tmux.conf` file is needed, as it doesn't exist. We can create this with `sudo touch /etc/tmux.conf`, or `sudo touch ~/.tmux.conf` for a user specific one.  

```bash
# Reload config file with CTRL + b + r
bind r source-file /etc/tmux.conf

# Activate mouse (Scrolling, selecting, re-sizing)
set -g mouse on
```

