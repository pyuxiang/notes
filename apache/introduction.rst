===============================================================================
Introduction
===============================================================================

Philosophy
==========

Thread-based MPM. Reverse proxy, load balancing.


Runtime execution
=================

Apache runtime revolves around loading configuration to achieve desired
server behaviour. It relies on the global configuration file, which contains
the relevant settings and includes other configuration files.
In particular, note that other configuration files are loaded using the
``Include`` statement.

.. sourcecode:: none
    :linenos:

    ServerRoot "/usr/local/apache"
    Mutex file:/var/lock/ default
    PidFile /var/lock/apache.pid
    Timeout 300

    ...

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

    ...

    Include /etc/config/apache/extra/apache-default-modules.conf
    <IfModule ssl_module>
    	SSLRandomSeed startup builtin
    	SSLRandomSeed connect builtin
    </IfModule>

    AccessFileName .htaccess
    <FilesMatch "^\.ht">
        Require all denied
        Satisfy All
    </FilesMatch>

    ...

    Include /etc/config/apache/extra/apache-ssl.conf
    Include /etc/config/apache/extra/apache-force-ssl.conf
    Include /etc/config/apache/extra/httpd-vhosts-user.conf
    Include /etc/config/apache/extra/httpd-ssl-vhosts-user.conf

Each setting is performed using XML-like tags that progressively apply more
specialised rules on a directory-level basis. This is the primary advantage of
Apache as opposed to Nginx.


Directory-level access
======================

Specific access to each directory via HTTP can be controlled either via
this global configuration file, or directory-level access files known as
``.htaccess`` (hypertext access).

There are benefits to either approach:

- Modifying the Apache configuration requires intervention by the
  server administrator, as well as restarting of the
  Apache service and is more liable to misconfiguration due to syntax errors,
  but it pays off in performance since the directory permissions are loaded
  only during startup time.
- Access controls can be offloaded to individual directory
  maintainers responsible for their own ``.htaccess`` files within their
  directory. Since this is a runtime protocol, prototyping can be easily
  performed without needing to restart the server, but performance is
  impacted since every parent directory must be queried for their ``.htaccess``
  file to determine the overall access level.

The use of ``.htaccess`` access controls is governed by the ``AllowOverride``
flag for each directory tag in the configuration files:

.. sourcecode:: none

    <Directory />
        Options FollowSymLinks
        AllowOverride None
        Require all denied
    </Directory>
    <Directory "/share">
        Options FollowSymLinks MultiViews
        AllowOverride All
        Require all granted
    </Directory>
    <Directory "/share/Web">
        Options FollowSymLinks MultiViews
        AllowOverride None
        AuthName "Protected"
        AuthType Basic
        AuthUserFile /etc/config/password.txt
        Require valid-user
    </Directory>

In the example above,

1. A blanket access ban is first applied to the whole root directory, before
   specific directories are opened up in subsequent directory configs.
2. The ``/share`` directory access configuration can be
   overridden by individual ``.htaccess`` within each (sub)directory using the
   ``AllowOverride All`` flag, and all users can access the directory by
   default with the ``Require all granted`` flag.
3. The ``/share/Web`` directory requires user authentication whose allowed
   users are located in the ``/etc/config/password.txt`` file.
   There are many forms of authentication, the recommended one being
   Basic (plaintext transmission of credentials)
   over HTTPS (with SSL certificate).

Note that ``AuthName`` is typically displayed together with the credentials
prompt during authentication.

Basic authentication
--------------------

The ``AuthUserFile`` is created using the ``htpasswd`` utility, which should
come with the ``mod_auth_basic`` module. In line with the previous example,

.. sourcecode:: none

    $ /usr/local/apache/bin/htpasswd -c /etc/config/password.txt USERNAME
    New password:
    Re-type new password:
    Adding password for user USERNAME
    $ cat /etc/config/password.txt
    USERNAME:$apr1$k1RaHS6X$/RL.sjUTN.adcC6ErcEfv1

A basic guide to authentication in Apache can be found in the
`Apache auth tutorial <https://httpd.apache.org/docs/2.4/howto/auth.html>`_.
