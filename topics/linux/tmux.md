# Basic

```bash
tmux                # Start tmux.
CTRL + B            # Command mode.
SHIFT + select      # Select/Paste text.
```

# Windows (tabs)

```bash
c          # New window.
numbers    # Change window.
,          # Name window.
w          # List windows.
.          # Move window. Asks for a number.
&          # Close window.
CTRL + D   # Close window.
```

# Panes (splits)

```bash
%        # Split horizontally (left/right).
\"       # Split vertically (top/bottom).
arrows   # Change pane. Resize if holding command.
x        # Close pane.
q        # Show pane numbers
space    # Toggle between layouts
```

# Scrolling

```bash
[          # Scrolling mode with arrows or PageUp/PageDown.
CTRL + C   # Exit scrolling mode.
```

# Mouse scrolling

In order to configurate tmux, a `.tmux.conf` file is needed, as it doesn't exist. We can create this with `sudo touch /etc/.tmux.conf`, or `sudo touch ~/.tmux.conf` for a user specific one.

**.tmux.conf**

```bash
# Activate mouse (Scrolling, selecting, re-sizing)
set -g mouse on

bind-key -T copy-mode-vi MouseDragEnd1Pane send -X copy-pipe-and-cancel "xclip -selection clipboard"
```

**IMPORTANT:** You must reload the configuration.

```bash
tmux source-file ~/.tmux.conf

# Inside tmux (CTRL + B + :)
source-file ~/.tmux.conf
```

# Setup script

Create a `tmux.sh` file to automate the setup.

```bash
# Start a new tmux session
tmux new-session -d  \; \

# Split the window vertically: left and right
tmux split-window -h  \; \

# Split the right pane vertically: left and right
tmux split-window -h  \; \

# Adjust the panes to be of equal width
tmux select-layout even-horizontal

# Run a script in the first pane (left)
tmux send-keys -t 0 'cd ~/project/app && npm run dev' C-m  \; \

# Run a script in the second pane (middle)
tmux send-keys -t 1 'cd ~/project/server/nodemon server.js' C-m \; \

# Move focus to the third pane (right)
tmux select-pane -t 2 \; \

# Attach to the tmux session to see the panes
tmux attach-session
```
