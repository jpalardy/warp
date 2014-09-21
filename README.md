
warp
====

Imagine if you could SSH somewhere by picking a hostname from a list.

Now you can:

![warp demo](https://raw.githubusercontent.com/jpalardy/warp/master/assets/warp.gif)

`warp` is a script that reads a file (`~/.warp`) and displays it in VIM. When
you press enter, you SSH to the hostname under the cursor.

If you select multiple lines, it opens cluster SSH (`csshX`).

Why?
----

If you have a bunch of servers you can SSH to, think about how you manage that now:

* you TYPE the hostname, maybe relying on bash completion?
* you create a BUNCH of aliases, with varying level of sophistication, based on how many hostnames?

Picking from a list solves all that.


How?
----

Create the file `~/.warp` and put one hostname per line.

Like this:

    example.com
    www@example.com

    # AMAZON

    -- production useast
    app10.useast1.ec2.example.com
    app11.useast1.ec2.example.com
    app12.useast1.ec2.example.com
    -- production uswest
    app10.uswest1.ec2.example.com
    app11.uswest1.ec2.example.com -- deprecated
    app12.uswest1.ec2.example.com

    # LOCAL

    tv
    router
    192.168.0.5 -- laptop

The simplest format is one hostname per line. When you press enter, only the
first column (`awk '{print $1}'`) is taken as the hostname.

This also means, if you DON'T press enter on a line, it can be whatever
you want: it can be blank or contain comments. This can greatly help with the
visual organization of the file. I like to put headers, separators and notes all
over the file.

What happens if you press enter on a line that's not a hostname? It will try to SSH there
and it won't work ... don't do that :-)

If you visually select (shift-v) multiple lines, cluster SSH (`csshX`) will be
used to SSH to them.

Any VIM movement commands will work, this is regular VIM after all. I recommend
searching with regular expressions, but using line numbers is good too.

VIM is started with the content of `~/.warp`, but you can modify the buffer
before making a selection -- changes will NOT be saved back to `~/.warp`. This
is useful if you want to SSH to multiple hosts that are not on lines following
each other: just slice and dice the file, put the lines together, add or modify
something, select them and press enter.

What about ports or other host-specific config?
-----------------------------------------------

I think the right solution for this is the put your host-specific config in
your `$HOME/.ssh/config`:

    Host somewhere
      Hostname somewhere.com
      Port 5656
      User bob
      # etc...

The syntax is simple, it goes with your other SSH config, and it configures
your other SSH-backed commands like scp, rsync, and cluster SSH.


Warp as an executable
---------------------

Put `warp` somewhere in your $PATH. This is simple but it won't be able to
modify your history: your bash history will say `warp` without indications of
where you warped to.

See below.


Warp as a bash script
---------------------

Add `source PATH/TO/warp` in your `.bashrc`. Now you can warp: reload your
shell, type `warp`, press enter. A function was added to your shell.

`warp` will modify the history to contain the SSH command (as if you typed it)
rather than `warp`.

