## Installation 

```
sudo apt install tmux -y
```

## Start and Exit 

```
tmux                                  // Start a new session
tmux new -s work                      // Start session named work
tmux ls                               // List sessions
tmux kill-session -t work             // Kill session work
tmux kill-server                      // Stop tmux server
exit                                  // Exit Current shell 
```

## Attach and Switch 

```
tmux attach                // Attach to the most recent session from the shell
tmux attach -t work        // Attach to session `work` from the shell
Ctrl+b  s                  // Choose a session from the session list
Ctrl+b  (                  // Switch to the previous session
Ctrl+b  )                  // Switch to the next session
Ctrl+b  d                  // Detach from the current session
```

## Prefix Key 

```
Ctrl+b      // Enter tmux command mode
Ctrl+b  ?   // Show key bindings
Ctrl+b  :   // open command prompt  
```

## Windows 

```
Ctrl+b c    // Create new window
Ctrl+b ,    // Rename current window
Ctrl+b &    // Kill current window
Ctrl+b n    // Next window
Ctrl+b p    // Previous window
Ctrl+b 0-9  // Switch to window by number
```

## Panes 

```
Ctrl+b %      // Split pane vertically
Ctrl+b "      // Split pane horizontally
Ctrl+b x      // Kill current pane  
Ctrl+b o      // Cycle panes
Ctrl+b q      // Show pane numbers
Ctrl+b ;      // Toggle last active pane
Ctrl+b z      // Zoom/unzoom pane
```

## Pane Navigation 

```
Ctrl+b ←/→/↑/↓         // Move between panes
Ctrl+b Ctrl+←/→/↑/↓    // Resize the current pane
Ctrl+b Alt+1           // Select the even-horizontal layout
Ctrl+b Alt+2           // Select the even-vertical layout
Ctrl+b Alt+5           // Select the tiled layout
```

## Copy Mode 

```
Ctrl+b [      // Enter copy mode
q             // Exit copy mode
Space         // Start selection
Enter         // Copy selection
Ctrl+b ]      // Paste buffer
```