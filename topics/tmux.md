# tmux

## Basic
```bash
tmux       # Start tmux.  
CTRL + B   # Command mode.
```

## Windows (tabs)
```bash
c          # New window.  
numbers    # Change window.  
,          # Name window.  
w          # List windows.  
.          # Move window. Asks for a number.  
&          # Close window.
CTRL + D   # Close window.
```

## Panes (splits)
```bash
%        # Split horizontally (left/right).   
\"       # Split vertically (top/bottom).  
arrows   # Change pane. Resize if holding command.  
x        # Close pane.  
q        # Show pane numbers
space    # Toggle between layouts
```

## Misc
```bash
[          # Scrolling mode with arrows or PageUp/PageDown. 
CTRL + C   # Exit scrolling mode.
```

# Setup

Create a `tmux.sh` file to automate the setup.

```bash
tmux new-session \; \
    send-keys 'echo "Pane 1"' C-m \; \
    send-keys 'cd /mnt/c/<user>/lab/posto/app && npm start' C-m \; \
    split-window -h -p 75 \; \
    send-keys 'echo "Pane 2"' C-m \; \
    send-keys 'cd /mnt/c/<user>/lab/posto/server && nodemon server.js' C-m \; \
    split-window -h -p 66 \; \
    send-keys 'echo "Pane 3"' C-m \; \
    send-keys 'echo <password> | sudo -S service mysql start && sudo mysql -u root -p<password>' C-m \; \
    split-window -h -p 50 \; \
    send-keys 'echo "Pane 4"' C-m \; \
    send-keys 'cd /mnt/c/<user>/lab/posto' C-m \; \
    # split-window -v \; \
    # select-pane -t 0 \; \
```

# Configuration
In order to configurate tmux, a `tmux.conf` file is needed, as it doesn't exist. We can create this with `sudo touch /etc/tmux.conf`, or `sudo touch ~/.tmux.conf` for a user specific one.  

```bash
# Reload config file with CTRL + b + r
bind r source-file /etc/tmux.conf

# Activate mouse (Scrolling, selecting, re-sizing)
set -g mouse on
```

