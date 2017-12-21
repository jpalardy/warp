
# warp

Imagine if you could SSH somewhere by picking a hostname from a list.

Now you can:

![warp demo](https://raw.githubusercontent.com/jpalardy/warp/master/assets/warp.gif)

`warp` is a script that reads a file (`~/.warp`) and displays it in VIM. When
you press ENTER, you SSH to the hostname under the cursor.

If you select multiple lines, it opens cluster SSH.


## Why?

How do you manage your servers now?

* do you TYPE the hostname, maybe relying on bash completion?
* do you create (and maintain) a BUNCH of aliases, one for each host?

Picking from a list solves all that.


## How?

Here's a sample `~/.warp` file:

    # format 1: one hostname per line
    example.com
    www@example.com

    # format 2: hostname:port
    anotherBox:2222                  # a comment

    # format 3: more than 1 column per line, taken as-is
    ssh anotherBox
    ssh -p 2222 anotherBox
    mosh anotherBox                  # doesn't have to be ssh...
    mosh -p 2222 anotherBox

Details:

- comments are trimmed, `#` and everything after
- if what's left is one column, the command is implied: `ssh`
- you can override the command with the `SSH` environment variable
- lines with more than 1 column are taken as-is

Comments:

The `.warp` file itself can contain empty lines, comments, headers, separators, etc ...
just don't press ENTER on lines you don't want to ssh to.

Any VIM movement commands will work, this is regular VIM after all. I recommend
searching with regular expressions, but using line numbers is good too.

You can edit the buffer before making a selection. Changes will NOT be
saved back to `~/.warp`.


## Cluster SSH

If you select (shift-v) multiple lines and press ENTER, the content of each line are passed together as the arguments of cluster SSH:

- on Linux, the executable is named `cssh`
- on MacOS, the executable is named `csshX`
- you can specify the executable name using the `MULTISSH` environment variable

If you want to SSH to multiple hosts that are NOT on lines following each
other: just slice and dice the buffer, put the lines together, add or modify
something, select them and press ENTER. Changes will NOT be saved back to `~/.warp`.


## Warp as an executable

Put `warp` somewhere in your $PATH. This is simple but it won't be able to
modify your history: your bash history will say `warp` without indications of
where you warped to.

See below.


## Warp as a bash script

Add `source PATH/TO/warp` in your `.bashrc`. Now you can warp: reload your
shell, type `warp`, press enter. A function was added to your shell.

`warp` will modify the history to contain the SSH command (as if you typed it)
rather than `warp`.


## Contributors

Thanks to the following people for helping me out:

* Daniel T ([@r0b0tbuilder](https://github.com/r0b0tbuilder))
* Daniel Morrison ([@dmorrison42](https://github.com/dmorrison42))
* David Chapman ([@dchapman1988](https://github.com/dchapman1988))
* Dalei ([@daleione](https://github.com/daleione))

