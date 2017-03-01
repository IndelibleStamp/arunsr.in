.. title: general linux
.. slug: general-linux
.. date: 2017-03-01 10:41:21 UTC+05:30
.. tags: linux, notes
.. category: linux
.. link: 
.. description: 
.. type: text

.. contents::

ccache notes
------------

Install from the repo as usual

.. code:: bash
          
    sudo yum install ccache -y

One way to set it up:

.. code:: bash

    bash-4.2 /bin$ cp ccache /usr/local/bin/
    bash-4.2 /bin$ ln -s ccache /usr/local/bin/gcc
    bash-4.2 /bin$ ln -s ccache /usr/local/bin/g++
    bash-4.2 /bin$ ln -s ccache /usr/local/bin/cc
    bash-4.2 /bin$ ln -s ccache /usr/local/bin/c++

Change timezone in centos:
--------------------------

.. code:: bash
          
          ln -s /usr/share/zoneinfo/Asia/Kolkata /etc/localtime

For debian:

.. code:: bash
          
          timedatectl set-timezone Asia/Kolkata

Install fonts in centos/linux:
------------------------------

System-wide :

.. code:: bash

          mkdir -p /usr/share/fonts/greatvibes

User only :

.. code:: bash

          mkdir ~/.fonts

Copy your font files in the appropriate folder and "register" them in
the system with:

.. code:: bash

          fc-cache -f -v

Linux date conversion (epoch to human readable)
-----------------------------------------------

Convert epoch time to human readable format:

.. code:: bash

          date -d @1445305686.222


Howto add swap space:
---------------------

.. code:: bash

    free
    dd if=/dev/zero of=/var/swap.img bs=1024k count=1000
    mkswap /var/swap.img
    swapon /var/swap.img
    free

Disable firefox's verification of extension signing
---------------------------------------------------

``xpinstall.signatures.required`` in ``about:config``

Firefox: open links in background tab always
--------------------------------------------

-  Open a new tab, enter ``about:config``
-  Search for browser.tabs.loadDivertedInBackground
-  Double click on '``false``\ ' to set 'Value' to '``true``\ '
-  Go to NewsBlur and open a story with 'o' and see it load in the
   background

(from newsblur's Goodies page)

If df shows no disk space even after deleting files, check this output:
-----------------------------------------------------------------------

.. code:: bash

          sudo /usr/sbin/lsof | grep deleted

-  Space will not be freed for the files there.
-  Restart those offending daemons to actually free the space up.

If you don't have lsof, just use this:

.. code:: bash

    find /proc/*/fd -ls | grep  '(deleted)'

Useful linux diagnostic commands:
---------------------------------

.. code:: bash

    uptime
    dmesg | tail
    vmstat 1
    mpstat -P ALL 1
    pidstat 1
    iostat -xz 1
    free -m
    sar -n DEV 1
    sar -n TCP,ETCP 1
    top

GNU/Screen scrollback:
----------------------

``Ctrl a Esc`` (then use ``Ctrl b/Ctrl f/Ctrl u/Ctrl d`` etc) and
``Esc`` to end

Quick fsck (solaris)
--------------------

.. code:: bash

          fsck -Fy ufs /dev/rdsk/c1d0s5

Debian - clean up orphaned files:
---------------------------------

.. code:: bash

          aptitude  remove --purge $(deborphan)

See filesystem usage:
---------------------

.. code:: bash

          /usr/bin/du --total --summarize --human-readable --one-file-system

GNU/Screen splitting windows
----------------------------

-  ``C-a V or C-a |`` split the screen vertically
-  ``C-a X`` remove/detach the current split
-  ``C-a S`` split horizontally
-  ``C-a tab`` cycle between windows

Tmux keybindings
----------------

-  ``Ctrl-b %`` (Split the window vertically)
-  ``Ctrl-b :`` "split-window" (Split window horizontally)
-  ``Ctrl-b o`` (Goto next pane)
-  ``Ctrl-b q`` (Show pane numbers, when the numbers show up type the
   key to goto that pane)
-  ``Ctrl-b {`` (Move the current pane left)
-  ``Ctrl-b }`` (Move the current pane right)

And here's my .tmux.conf

.. code:: bash

    set -g prefix C-a
    unbind C-b
    bind C-a send-prefix

    set -g default-terminal "xterm-256color"

    set -g history-limit 10000
    set -g set-titles-string "#T"

    unbind %
    bind | split-window -h
    bind - split-window -v

Colour in terminals
-------------------

.. code:: bash

    arunsrin@ARUNSRIN-G2CA5 MINGW64 ~
    $ printf "\033[32mhi\033[0m"
    hi

-  ``\033`` is Escape
-  So ``Escape + 3 + 2 + m`` tells the terminal that everything from
   this point onwards is in green.
-  And ``Escape + [ + 0 + m`` reverts it back to normal

-  These are some sequences:

.. code:: bash

    Sequence What it Does
    ESC[1m Bold, intensify foreground
    ESC[4m Underscore
    ESC[5m Blink
    ESC[7m Reverse video
    ESC[0m All attributes off

Bash Stty: Coredump etc
-----------------------

.. code:: bash

          Ctrl \

or

.. code:: bash

          kill -SIGQUIT <pid>

-  Override it with
-  ``stty quit <some-binding>``
-  similarly for that age-old backspace not deleting a character
   problem:
-  ``stty erase ^h``
-  To see the current terminal capabilities, run:
-  ``stty -a``

Fix for xargs errors when filenames contain spaces
--------------------------------------------------

-  ``find`` has a print0 option that uses null characters instead of
   :raw-latex:`\n`as separators.
-  ``xargs`` has a -0 option that uses the same separator when working
   on the args. So:

.. code:: bash

          find . -name -print0 \| xargs -0 ls -l

Bash faster navigation with cdpath
----------------------------------

.. code:: bash

    export CDPATH=:$HOME:$HOME/projects:$HOME/code/beech

-  cd'ing to a folder first looks at CWD, then rest of CDPATH

Find
----

with date filters

-  ``find . -ctime -3`` # created in the past 3 days
-  ``find . -ctime +3`` # older than 3 days
-  ``find . -ctime 3`` # created exactly 3 days back
-  ``find . -ctime +3 -ctime -5`` # created 3 - 5 days back
-  ``find . -newer /tmp/somefile`` # see somefile's timestamp and show
   files newer than it
-  works great in conjunction with:
-  ``touch 0607090016 /tmp/somefile`` #i.e. 7th june, 9:00 am, 2016
-  ``find . -maxdepth 1 -type d -ctime +38 -exec rm -rf  {} \;`` delete
   all folders older than 38 days back.
-  don't use atime much: every directory access changes its atime, so
   when find traverses through it, the inode's atime entry gets updated.

File formatting, wrapping etc
-----------------------------

-  huh, who knew this existed:

.. code:: bash

          cat <some-verbose-output> \| fold -70

-  ``fold -s`` folds at whitespace

-  Also look at the ``fmt`` command, which seems similar to emacs'
   ``fill-paragraph``.

-  ``pr`` gives a pretty display with margins, headers, and page
   numbers.

Deleting files with odd names
-----------------------------

-  There's more than one way. Here's one: find the inode with ``ls -i``,
   then delete with:

.. code:: bash

          find -inum <inode-number> -exec rm -i {} 

See whitespace with cat
-----------------------

-  use this:

.. code:: bash

          cat -v -t -e <somefile>

-  ``-e``: Add a trailing ``$`` at the end of a line.
-  ``-t``: Show tabs as ``^I``

Stat command: see inode information
-----------------------------------

-  The inode holds the address in the filesystem, access permissions,
   ctime/mtime etc

.. code:: bash

    arunsrin@ARUNSRIN-G2CA5 MINGW64 ~
    $ stat ntuser.ini
      File: ‘ntuser.ini’
      Size: 20              Blocks: 1          IO Block: 65536  regular file
    Device: a4b221d6h/2763137494d   Inode: 562949953421373  Links: 1
    Access: (0644/-rw-r--r--)  Uid: (1233064/arunsrin)   Gid: (1049089/ UNKNOWN)
    Access: 2015-07-21 18:57:13.142410100 +0530
    Modify: 2010-11-21 08:20:53.336035000 +0530
    Change: 2016-06-06 09:18:05.239486700 +0530
     Birth: 2015-07-21 18:57:13.142410100 +0530

    arunsrin@ARUNSRIN-G2CA5 MINGW64 ~
    $

-  If the filename is odd and you can't paste it easily in the terminal,
   just try

.. code:: bash

          ls -il

Bash debugging
--------------

-  Run the script with ``-xv`` in the shebang:

.. code:: bash

    #!/bin/bash -xv
    # do something

Bash suppress echo (for reading passwords)
------------------------------------------

In bash, while reading input from the user, if you want to suppress the
echo on the screen (for sensitive inputs like passwords), do this:

.. code:: bash

    stty -echo
    read SECRETPASSWD
    stty echo

ngrep
-----

Try this:

.. code:: bash

          sudo ngrep -d any <word> -q

- ``-d any`` listens on any interface
- ``-q`` is quiet mode so those ``#``'s don't show.

Pretty-print json
-----------------

.. code:: bash

          cat somefile.json | python -m json.tool
