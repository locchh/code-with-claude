# tmux Cheatsheet

| No | Command | Description |
|----|---------|-------------|
| 1 | `tmux new -s <session_name>` | Create new session with name |
| 2 | `Ctrl + b, d` | Detach from current session |
| 3 | `tmux a` | Attach to last session |
| 4 | `tmux ls` | List all sessions |
| 5 | `tmux kill-session -t <session_name>` | Kill specific session |
| 6 | `Ctrl + b, %` | Split window vertically |
| 7 | `Ctrl + b, "` | Split window horizontally |
| 8 | `Ctrl + b, ← → | ↑ ↓` | Navigate between panes |
| 9 | `Ctrl + b, q` | Show pane numbers |
| 10 | `Ctrl + b, q + number` | Go to pane by number |
| 11 | `Ctrl + b, Ctrl + ← → | ↑ ↓` | Resize pane |
| 12 | `Ctrl + b, Alt + number` | Go to window by number |
| 13 | `Ctrl + b, c` | Create new window |
| 14 | `Ctrl + b, ,` | Rename window |
| 15 | `Ctrl + b, w` | List windows |
| 16 | `Ctrl + b, x` | Kill current pane |
| 17 | `Ctrl + b, &` | Kill current window |