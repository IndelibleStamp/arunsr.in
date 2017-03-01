.. title: package management
.. slug: package-management
.. date: 2017-03-01 10:41:45 UTC+05:30
.. tags: linux, notes, rpm, yum, centos
.. category: linux
.. link: 
.. description: 
.. type: text

.. contents::

Sort RPMs by size
-----------------

.. code:: bash

          rpm -qa --queryformat '%{size} %{name}\n' | sort -rn | more

Extract rpm into current folder instead of installing:
------------------------------------------------------

.. code:: bash

          rpm2cpio boost-system-1.53.0-23.el7.x86_64.rpm | cpio -idmv

Trace a binary or file to the RPM that installed it:
----------------------------------------------------

.. code:: bash

          yum whatprovides /usr/lib64/libdbus-c++-1.so.0

or this:

.. code:: bash

          rpm -qf /usr/lib64/libdbus-c++-1.so.0

Yum/dnf revert
--------------

-  if a yum remove wiped out several packages, do this:
-  ``dnf history`` # note the id of the bad removal here
-  ``dnf history undo 96``
-  yum/dnf will reinstall all the packages that were removed in that id.

Dependencies of a package
-------------------------

-  This command shows what other packages need the queried package:

.. code:: bash

          repoquery --whatrequires libunwind

-  This command shows what other packages need to be installed for a
   queried package:

.. code:: bash

          yum deplist nginx


