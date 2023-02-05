# what are panes

so the same way a GUI has windows, tmux has panes!

## creating a pane

Two easiest ways to make panes are with verticle and horizontal splits

### verticle split 

`ctrl+b %`

### horizontal split

`ctrl+b "`

## navigate panes

use `ctrl+b [arrow key]`

## resizing a pane

go to the prompt using `ctrl+b :`

the type `resize-pane -[flag] [lines]`

The flags are:

- U for up
- D for down
- L for left
- R for right

\[lines\] represents the number of lines to increase by