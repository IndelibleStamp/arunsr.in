.. title: systemd
.. slug: systemd
.. date: 2017-03-01 10:41:34 UTC+05:30
.. tags: linux, notes, systemd
.. category: linux
.. link: 
.. description: 
.. type: text

.. contents::

Flush old logs in journalctl
----------------------------

-  By date or by size:

.. code:: bash

    sudo journalctl --vacuum-time=2d
    sudo journalctl --vacuum-size=500M

Tail journalctl
---------------

-  ``journalctl -f``

Store logs on disk
------------------

(from
http://unix.stackexchange.com/questions/159221/how-display-log-messages-from-previous-boots-under-centos-7)

On CentOS 7, you have to enable the persistent storage of log messages:

.. code:: bash

    # mkdir /var/log/journal
    # systemd-tmpfiles --create --prefix /var/log/journal
    # systemctl restart systemd-journald

Otherwise, the journal log messages are not retained between boots. This
is the default on Fedora 19+.

