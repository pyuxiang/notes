===============================================================================
Apache
===============================================================================

Sources:

https://www.digitalocean.com/community/tutorials/how-to-configure-the-apache-web-server-on-an-ubuntu-or-debian-vps


Installing Apache

sudo apt-get update # what is this for?
sudo apt-get install apache2 # why apache2?





QNAP specific

Apache config files are in
``/etc/config/apache``. The configuration directory is a symbolic(?) link to
``/mnt/HDA_ROOT/.config``. No idea what ``HDA_ROOT`` is.

QNAP's version is a little different.

.. code-block::

    [/mnt/HDA_ROOT/.config/apache] # ls
    apache.conf       apache.conf.orig  extra/            mime.types        php-fpm.conf
    apache.conf.bak   apache.conf.tmp   magic             original/

But the configuration file is pretty much the same.
This `link <https://www.digitalocean.com/community/tutorials/how-to-
configure-the-apache-web-server-on-an-ubuntu-or-debian-vps>`_
explains some global configurations in the ``apache.conf`` file.

Ignoring everything else first, this is the line I'm looking for:

.. code-block::

    DocumentRoot "/share/Web"
    <Directory />
            Options FollowSymLinks
            AllowOverride None
            Require all denied
    </Directory>
    <Directory "/share/Web">
            Options FollowSymLinks MultiViews
            AllowOverride All
            Require all granted
    </Directory>
    Include /etc/config/apache/extra/apache-default-modules.conf
    <IfModule headers_module>
            Header always append X-Frame-Options SAMEORIGIN "env=!share-iframe"
            Header always edit Set-Cookie ^(.*)$ $1;HttpOnly
    </IfModule>
    <IfModule dir_module>
                    DirectoryIndex index.html index.htm index.php
    </IfModule>

According to
https://forum.qnap.com/viewtopic.php?t=100689,
the root directory is controlled by the Apache/PHP configuration already
defined during setup. Easiest method to override it is to simply let
the symbolic link ``/share/Web@`` point to a separate directory, i.e.
``ln -s /path/to/src /share/Web``.

Schtasks success23455




To figure out how to enable MOD_WSGI now...


PASSWORD AUTHENTICATION

``.htaccess`` files allow individual pages to require authentication,
but increases number of checks required at each subdirectory to requested file.


https://httpd.apache.org/docs/2.4/howto/auth.html

.. code-block::

    [/usr/local/apache] # ./bin/htpasswd -c /usr/local/apache/passwd/passwords USERNAME
    New password:
    Re-type new password:
    Adding password for user USERNAME
    [/usr/local/apache] # cat passwd/passwords
    USERNAME:$apr1$k1RaHS6X$/RL.sjUTN.adcC6ErcEfv1

You can then add the authentication in the Apache configuration file, known
as ``apache.conf`` in QNAP:

.. code-block::

    <Directory "/share/Web">
        Options FollowSymLinks MultiViews
        AllowOverride All
        AuthName "Protected"
        AuthType Basic
        AuthUserFile /etc/config/password.txt
        Require valid-user
    </Directory>


Now trying out how to add subdomain. Is it the role of DNS or server to
determine target? Turns out, should be DNS, because the following doesn't work:

.. code-block::

    $ ping qoptics.pyuxiang.com
    Ping request could not find host qoptics.pyuxiang.com.
    Please check the name and try again.

test
