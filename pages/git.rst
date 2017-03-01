.. title: git
.. slug: git
.. date: 2017-03-01 11:12:45 UTC+05:30
.. tags: git
.. category: git
.. link: 
.. description: 
.. type: text

.. contents::

Color coded git output
======================

.. code:: bash

          git config --global color.ui true

Handling merge request in gitorious (and maybe others):
=======================================================

-  Changes will be pulled into a local branch for review. If the changes
   are approved they will be merged back to the master and pushed into
   the remote repository.
-  Check out a new branch for integration

.. code:: bash

          git checkout -b merge-requests/1<code>

-  Fetch the merge request into this branch

.. code:: bash

          git pull git://gitorious.org/project/project.git refs/merge-requests/1

-  Show the commits, assess they are okay

.. code:: bash

          git log --pretty=oneline --abbrev-commit master..merge-requests/1</code>

-  Apply the changes to your branch

.. code:: bash

    git checkout master
    git merge merge-requests/1
    git push origin master

If git has trouble getting code from github etc:
================================================

.. code:: bash

          export GIT_SSL_NO_VERIFY=true

Git objects:
============

-  Tree
-  Blob
-  Commit
-  Tag

Reverting a reverted commit in git
==================================

If you've reverted a recent commit with:

.. code:: bash

          git reset HEAD^

You can undo the revert with this:

.. code:: bash

          git reset HEAD@{1}

Git stash:
==========

-  First do a ``git add`` (if its a new file to be tracked), then
   ``git stash``
-  Check with ``git stash list``
-  Reapply with ``git stash apply``
-  Otherwise use ``git stash pop`` to recover the files and discard the
   stash
-  Creating a branch from a stash:

.. code:: bash

          git stash branch testchanges

Git commits that aren't in master
=================================

-  To see commits that have not yet merged to master:

.. code:: bash

          git log --no-merges master..

Stash: checkout a pull request
==============================

-  From here: https://gist.github.com/hongymagic/6339056

First add this to ``.git/config`` under the ``origin`` section:

.. code:: bash

        fetch = +refs/pull-requests/*:refs/remotes/origin/pull-requests/*

Then fetch the pull requests:

.. code:: bash

          git fetch origin

Then checkout the one you want:

.. code:: bash


          git checkout pull-requests/1000/from

Git rebase vs normal pull
=========================

-  Instead of a normal pull, try this:

.. code:: bash

          git pull --rebase origin master

-  'The ``--rebase`` option tells Git to move all of Mary's commits to
   the tip of the master branch after synchronising it with the changes
   from the central repository.'
-  From here:
   https://www.atlassian.com/git/tutorials/comparing-workflows/centralized-workflow
-  This removes the superfluous 'merge commit' that comes up normally.
-  After fixing a merge conflict:

.. code:: bash

          git add <some-file>= ``git rebase --continue``

-  To abort:

.. code:: bash

          git rebase --abort

-  Finally:

.. code:: bash

          git push origin master

Git log on a file
=================

.. code:: bash

          git log -p filename

actually do this:

.. code:: bash

          git log --follow filename

Submodules
==========

-  To update all submodules:

.. code:: bash

          git submodule update --init --recursive

-  To fetch the latest code from a submodule:

.. code:: bash

    cd <submodule-folder>
    git pull
    cd ..
    git commit -am "bumping up submodule version"

Then merge the code. The next time the parent repository is pulled,
updating the submodule will get the latest commit in it.

Working with remotes
====================

-  Changing a remote's name

.. code:: bash

          git remote origin set-url http://some-other-url\

-  Adding a remote

.. code:: bash

          git remote add newremote http://newremote-url\

-  Then as usual push/pull to and from these remotes

.. code:: bash

          git pull origin master
          git push newremote master
