
warp
====

A bash script to SSH from a list of hostnames.

What?
-----

Imagine if you could make a list of hostnames, and SSH to them by selecting one
with a list picker.

Now you can.

![warp demo](https://raw.githubusercontent.com/jpalardy/warp/master/assets/warp.gif)

`warp` is a script that reads a file (`~/.warp`) and displays it in VIM. When
you press enter, you SSH to the hostname under the cursor.

If you select multiple lines, it opens cluster ssh (`csshX`).

Why?
----

If you have a bunch of servers you can SSH to, think about how you manage that now:

* you DO NOT want to have to type the hostnames
* you DO NOT want to create a bunch of aliases

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


Details:

* some lines are not hostnames, just don't select them and you won't SSH there
* same with blank lines
* you can put comments after the hostname, only the first column (`awk '{print $1}'`) is taken as the hostname
* if you visually select multiple lines, cluster ssh (`csshX`) will be used to SSH to them
* any VIM movement commands will work, this is regular VIM after all. I recommend searching with all the regular expression goodness but using line numbers is good too

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

