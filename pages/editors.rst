.. title: editors
.. slug: editors
.. date: 2017-03-15 09:49:39 UTC+05:30
.. tags: text, emacs, vim, editors
.. category: 
.. link: 
.. description: 
.. type: text

Org-mode Keybindings
====================

Basic keybindings
-----------------

-  ``C-c C-n`` and ``C-c C-p`` to cycle between headings.
-  ``TAB`` on a heading to expand/collapse.
-  ``M-up`` and ``M-down`` to reorder sections.
-  ``M-left`` and ``M-right`` to change the level of a heading.
-  ``M-RET`` inside a list to create a new bullet.

   -  ``TAB`` in a new bullet to indent it.
   -  ``S-left`` and ``S-right`` to change the bullet-style.

Checkboxes [2/3]
----------------

-  [ ] ``M-S-RET`` gives a checkbox.
-  [X] ``C-c C-c`` checks it.

   -  [X] ``TAB`` for subdivisions.
   -  [X] When all subtasks are checked, so is the main one.

-  [X] A trailing [] in the line preceding a list of checkboxes contains
   a summary (2/3 in this case).

Publishing/Exporting
--------------------

-  ``C-c C-e`` for everything.

   -  ``h o`` exports to html.
   -  ``#`` brings up common templates.

Org-mode code blocks
====================

-  Awk, C, R, Asymptote, Calc, Clojure, CSS, Ditaa, Dot, Emacs Lisp,
   Forth, Fortran, Gnuplot, Haskell, IO, J, Java, Javascript, LaTeX,
   Ledger, Lilypond, Lisp, Makefile, Maxima, Matlab, Mscgen, Ocaml,
   Octave, Org, Perl, Pico Lisp, PlantUML, Python, Ruby, Sass, Scala,
   Scheme, Screen, sh, Shen, Sql, Sqlite, ebnf2ps.

Set font in gvim permanently:
=============================

-  Change it for the current session and verify what it is set as with
   this:

.. code:: bash
          
          :set guifont?

-  Copy the string and add it to .vimrc like so:

.. code:: bash
          
          set guifont=Hack:h9:cANSI

Vim tips:
=========

-  delete trailing whitespace:

.. code:: bash
          
          :%s: *$::

-  pull onto search line:

.. code:: bash
          
          / CtrlR CtrlW

-  open file name under cursor:

.. code:: bash

          gf

-  increment/decrement number under cursor:

.. code:: bash

          CtrlA/CtrlX


