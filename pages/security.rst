.. title: security
.. slug: security
.. date: 2017-03-15 09:49:30 UTC+05:30
.. tags: security, openssl, certificates
.. category: 
.. link: 
.. description: 
.. type: text

.. contents::

Things that go in a certificate
===============================

.. code:: bash

    Subject: CN the certificate owner's common name
    Subject: E  the certificate owner's email address
    Subject: T  the certificate owner's locality
    Subject: ST the certificate owner's state of residence
    Subject: O  the organization to which the certificate owner belongs
    Subject: OU the name of the organizational unit to which the certificate owner belongs
    Subject: C  the certificate owner's country of residence
    Subject: STREET the certificate owner's street address
    Subject: ALL    the certificate owner's complete distinguished name
    Serial  the certificate's serial number
    SignatureAlg    the algorithm used by the Certificate Authority to sign the certificate
    BeginDate   the date at which the certificate becomes valid
    EndDate the date at which the certificate becomes invalid
    PublicKey   the certificate's public key
    FriendlyName    the certificate's friendly name

Create a self signed certificate and key
========================================

This is done as -

-  Create a private key - using RSA

.. code:: bash

          openssl genrsa -out privkey.pem 1024

-  Create a self signed certificate

.. code:: bash

          openssl req -new -x509 -key privkey.pem -out cacert.pem -days 1095

selinux example:
================

.. code:: bash

          ls -Z file1 -rwxrw-r-- user1 group1
	  -rwxrw-r-- user1 group1 unconfined_u:object_r:user_home_t:s0 file1

-  SELinux contexts follow the SELinux user:role:type:level syntax.
-  Use the ``ps -eZ`` command to view the SELinux context for processes
-  and ``id -Z`` for users
-  ``seinfo -r`` (part of setools-console): shows all available user
   roles: such as guest, unconfined, webadm, sysadm, dbadm, etc.

also see ``/etc/selinux/targeted/context/users``

decrypt an openssl key:
=======================

.. code:: bash

    openssl rsa -in ssl.key -out decrypted.ssl.key 
    sudo cp ssl.crt /etc/ssl/certs/indeliblestamp.com.pem 
    sudo cp decrypted.ssl.key /etc/ssl/private/indeliblestamp.com.key
    sudo cp sub.class1.server.ca.pem /etc/ssl/certs/sub.class1.server.ca.pem

SELinux/Nginx error
===================

``sudo cat /var/log/audit/audit.log | grep nginx | grep denied`` shows
something like this:

.. code:: bash

          type=AVC msg=audit(1445306182.317:301): avc: denied { name_connect } for pid=5939 comm“nginx” dest=4374 scontext=system_u:system_r:httpd_t:s0 tcontext=system_u:object_r:unreserved_port_t:s0 tclass=tcp_socket

Someone found that running the following commands fixed their issue:

.. code:: bash

          sudo cat /var/log/audit/audit.log | grep nginx | grep denied | audit2allow -M mynginx

          sudo semodule -i mynginx.pp

Or something like this:

.. code:: bash

          chcon -Rt httpd_sys_content_t /srv/www/myapp/

to change the security context of the directory recursively so nginx
will be allowed to serve it. Followed by:

.. code:: bash

          ``setsebool -P httpd_can_network_connect 1``

SElinux Apache static html
==========================

-  Change the context of the file:

.. code:: bash
          
          sudo chcon -R -v -t httpd\ :sub:`sysrwcontentt` index.html

-  This might have happened because we copied a file from ~ to
   ``/var/www``, which caused it to retain its original context.

Nmap one liners
===============

-  Port scan, os detection:

.. code:: bash

          nmap -sS -P0 -sV -O 192.168.0.58

-  All active IPs in a network

.. code:: bash
   
          nmap -sP 192.168.0.\*

-  Ping a range of IPs

.. code:: bash

          nmap -sP 192.168.0.2-254

-  Find unused IPs in a subnet

.. code:: bash

          nmap -T4 -sP 192.168.0.0/24 && egrep "00:00:00:00:00:00" /proc/net/arp

Make a password in linux (without adding the user)
==================================================

-  ``whois`` package: provides ``mkpasswd``

.. code:: bash

          mkpasswd --method=SHA-512

IPtables port forwarding
========================

Use case: make tomcat on port 8443 listen on port 443.

.. code:: bash

          sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080

This will forward all traffic coming in on port 443 to the tomcat server
listening on 8443.

(picked from here:
https://mihail.stoynov.com/2011/04/04/howto-start-tomcat-on-port-80-without-root-privileges/)

To view, the usual ``-L`` and ``-F`` won't show anything. Instead, use:

.. code:: bash

          iptables -L -t nat
          iptables -F -t nat

Open firewall ports in centos:
==============================

.. code:: bash

          firewall-cmd --permanent --add-port=5672/tcp
          firewall-cmd --reload

Centos firewall commands
========================

.. code:: bash

    firewall-cmd --state
    firewall-cmd --get-zones
    firewall-cmd --list-all-zones
    firewall-cmd --get-default-zone
    firewall-cmd --list-services  # currently enabled in this zone
    firewall-cmd --get-services   # all
    firewall-cmd --add-service=https --permanent
    firewall-cmd --add-service=http --permanent

ssh-agent
=========

-  You start an ssh-agent by running something like:

.. code:: bash

          eval `ssh-agent`

-  You can then feed it keys, with ssh-add like this:

.. code:: bash

          ssh-add /home/test/.ssh/id_rsa

or, if your key is in the default location, you can just do:
``ssh-add``

or just put this in ``.bashrc``:

.. code:: bash

    if [ -z "$SSH_AUTH_SOCK" ] ; then
      eval `ssh-agent -s`
      ssh-add
    fi

but this prompts for the passphrase the first time it is invoked. so do
this instead:

.. code:: tcl

    #!/usr/bin/expect -f
    spawn ssh-add /home/user/.ssh/id_rsa
    expect "Enter passphrase for /home/user/.ssh/id_rsa:"
    send "passphrase\n";
    interact

Apache redirect http to https
=============================

.. code:: bash

    NameVirtualHost *:80
    <VirtualHost *:80>
       ServerName mysite.example.com
       DocumentRoot /usr/local/apache2/htdocs 
       Redirect permanent / https://mysite.example.com/
    </VirtualHost>

Letsencrypt notes
=================

.. code:: bash

    sudo dnf install httpd -y
    sudo dnf install mod_ssl -y
    sudo systemctl start httpd
    sudo systemctl enable httpd

-  Add ``ServerName`` and a ``VirtualHost`` at a minimum
-  now run ``letsencrypt-auto`` and fill out the stuff

.. code:: bash

    sudo cp /etc/letsencrypt/options-ssl-apache.conf /etc/httpd/conf.d
    sudo systemctl restart httpd

-  To renew:

.. code:: bash

          letsencrypt-auto renew


