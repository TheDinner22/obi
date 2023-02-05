# what is

TMUX is a terminal session manager? IDK what that means yet but heres some basic stuff

# create a new session

`tmux new` creates a new session and you can type exit to destroy it or detatch to close it.

# LS

`tmux ls` will print all of your current sessions

# getting back into a session

So once you have detached from a session but want to get back into it.

1. get the id with `tmux ls`
2. call `tmux attach-session -t {id}`

# naming sessions

so you could lookup the id of a session to reattatch to it every single time or you could name it! 

## creating a named session

`tmux new -s [name of session]` will create a new session with the given name.

The command prefix is `ctl + b` by default.

## attaching to a named session

`tmux a -t [name of session]` can be used to then attach to the named session