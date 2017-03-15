.. title: networking
.. slug: networking
.. date: 2017-03-15 09:49:23 UTC+05:30
.. tags: networking
.. category: 
.. link: 
.. description: networking notes
.. type: text

.. contents::

Curl tips
=========

-  Show headers with ``-I`` and change request type with ``-X``. e.g.

.. code:: bash

          curl -I -X DELETE http://localhost/blah

-  To add a header in the outgoing request, use ``-H``:

.. code:: bash

          curl --header "X-MyHeader: 123" www.google.com

-  Follow redirects with ``-l``
-  Disable security check with ``-k``

If more than 10 telnet sessions to a server fail, check this flag:
==================================================================

``per_source = 10`` in ``/etc/xinetd.d/telnet`` or ``/etc/xinetd.conf``

Start xinetd with debugs turned on:
===================================

.. code:: bash

          /usr/sbin/xinetd -f /etc/xinetd.conf -d

Check duplicate ip with arping
==============================

.. code:: bash

    bash-4.2 ~$ arping 192.168.0.58 -D -c 3 -I ens32
    ARPING 192.168.0.58 from 0.0.0.0 ens32
    Unicast reply from 192.168.0.58 [18:E7:28:2E:92:9C]  1.747ms
    Sent 1 probes (1 broadcast(s))
    Received 1 response(s)
    bash-4.2 ~$ echo $?
    1
    bash-4.2 ~$ 

-  exit status of 0 confirms a duplicate ip

Test tcp connections with nc
============================

(from
http://unix.stackexchange.com/questions/73767/how-to-check-whether-firewall-opened-for-a-port-but-not-listening-on-the-port)

.. code:: bash

          nc -vz targetServer portNum

For example:

.. code:: bash

    bash-3.2 ~$ nc -vz erdos 22
    Connection to erdos 22 port [tcp/ssh] succeeded!
    bash-3.2 ~$ 

