.. title: other study notes
.. slug: other-study-notes
.. date: 2017-03-01 10:56:06 UTC+05:30
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

.. contents::


TCP
---

-  Use ``SO_REUSEADDR`` when stopping/starting servers: the OS will keep
   a socket alive for ~4 minutes after it's closed in case it has to
   retransmit FINs/ACKs.
-  Deadlocks occur if the OS buffers fill up in both ends. e.g. client
   send blocks of data, server has a ``recv(1024)`` and processes and
   sends data (say, 1024), but client only has a recv of say, 10. Then
   his buffer gets filled up since he's getting a lot more data than he
   can handle. so the server's sends stop working. similarly the
   server's recv fills up ...
-  Either can call ``socket.shutdown``, e.g. if you're a client who's
   finished sending data and wants to notify this. The connection stays
   open so he can continue getting data. ``shutdown``'s flags will stop
   reads, writes, or both.
-  Address families: pretty much always ``AF_INET``. ``AF_UNIX`` is for
   local file sockets. bluetooth etc also exist. Oh ``AF_INET6`` also
   exists.
-  Socket type: ``SOCK_DGRAM`` (2) and ``SOCK_STREAM`` (1). (each
   address family has its own udp/tcp equivalents under the dgram/stream
   types)
-  Last field is protocol which can be zeroed/ignored. ``IPROTO_TCP`` is
   6 and ``IPROTO_UDP`` is 17. but we can infer it from the socket type
   above so we don't need to set it each time.

-  To avoid v4/v6 and other binding confusions, use
   ``getaddrinfo(host,port)`` : it returns FTPCA (family, type,
   protocol, canonical name and address)

-  Use ``socket.getservbyname(53)`` to see port->service mappings.
-  Similarly ``socket.gethostbyname('abc.com')`` or
   ``gethostbyaddr('1.2.3.4')``
-  So self ip address is ``socket.gethostbyname(socket.getfqdn())``

Unicode in DNS:
---------------

-  RFC 3492 specifies the IDNA codec that maps a unicode hostname to an
   ascii representation.
-  The lookup is performed for the encoded ascii string only.

UTF-8
-----

-  utf-8: 1-4 bytes. use setlocale() to switch encodings

example of unicode encoding:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  character "¢"= code point U+00A2 = 00000000 10100010 → 11000010
   10100010 → hexadecimal C2 A2
-  explanation: if the actual code is 00000000 10100010, then the
   representation starts with a 11 (to show that 2 bytes are needed to
   represent this character), followed by the data. The continuation
   bytes always start with a 10.

example 2:
~~~~~~~~~~

-  The following string contains 4 utf-8 characters:

.. code:: bash

    "\xD4\xBC\xF0\x9D\x90\x84\x45\xC6\xAC\x00"

-  D4 converted to Hex is 11010100 which tells us (from the first 2
   bits) that 2 bytes take up this character. similarly F0 == 11110000
   which takes 4 bytes to specify the next character, and so on.
-  Do not modify strings directly in C (the compiler may store multiple
   identical string literals in the same address, so modifying one will
   affect the other)

Levenshtein Distance
--------------------

-  Used in fuzzy searching (e.g. 'git lgo' which autocorrects and
   recommends 'log')
-  Used to measure the difference between two strings

Linux's CFS: Completely Fair Scheduler
--------------------------------------

-  On a single cpu, the available cpu cycles are divided among all the
   threads in proportion to their weights.
-  The weight == priority == niceness.
-  Threads are organized in a runqueue (implemented as a red-black
   tree).
-  A thread exceeding its timeslice is pre-empted and the next one is
   given a slice.
-  In multicore systems, each core has its own runqueue.
-  But this may not always be fair (since one core may run one
   low-priority thread while the other may run several high-priority
   threads, inefficiently)
-  So linux has a load balancer that periodically keeps the queus in
   balance.
-  Load balancing is a costly operation (both computation and
   commuication operations are expensive) so it is kept at a minimum if
   possible.

Memory layout in Linux
----------------------

-  32-bit: 3:1 ratio: out of 4gb, 3gb is for users and 1gb for kernel
-  64-bit: 1:1: out of 128TB, 64TB is for users and 64 for kernel.
-  So the kernel memory starts at 0xffff80000000<snip>000 in 64-bit, and
   0xc000000 in 32-bit.
-  For a user process, Stack size is 8mb by default (see ulimit -s).
-  Stack occupies top of the address space and grows down.
-  The botttom of the stack contains env variables, prog name and
   \*\*args
-  Below the stack is memmap which has stuff linked dynamically and
   mapped by the kernel at runtime.
-  Then is the heap.
-  Then there's bss/data/program text.
-  text is usually read-only.
-  stack and data are non-executable (to prevent (partially) overflows).


