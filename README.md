
warp
====

A bash script to SSH from a list of hostnames.

What?
-----

Imagine if you could make a list of hostnames, and SSH to them by selecting one
with a list picker.

Now you can.

GIF GOES HERE

`warp` is a script that reads a file (`~/.warp`) and displays it in VIM. When
you press enter, you ssh to the hostname under the cursor.

Why?
----

You DO NOT want to have to type the hostnames. You DO NOT want to create a
bunch of aliases.

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


Notes:

* some lines are not hostnames, just don't select them and you won't SSH there
* same with blank lines
* you can put comments after the hostname, only the first column (`awk '{print $1}'`) is taken as the hostname

Put `warp` somewhere in your $PATH.

Add `source PATH/TO/warp` in your `.bashrc`. Now you can warp: reload your
shell, type `warp`, press enter.

Any VIM movement commands will work, this is regular VIM after all. I recommend
searching with all the regular expression goodness but using line numbers is
good too.

