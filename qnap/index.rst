===============================================================================
QNAP
===============================================================================

Listing configuration stuff for QNAP NAS.

Installing Python
=================

Python 2 is already shipped with QNAP. Python 3 on the other hand must be
installed from the App Center (not sure what package manager used).
This `QNAP forum thread <https://forum.qnap.com/viewtopic.php?t=152555>`_
is a good guide.

1. Install Python 3 from App Center
2. Configuration script to add environment variables for Python 3 to shell
   found at ``/etc/profile.d/python3.bash``, which is a symbolic link to
   ``/share/CACHEDEV1_DATA/.qpkg/Python3/python3.bash``. Run it, i.e.
   ``. /etc/profile.d/python3.bash``
3. Run Python 3 using ``python3``

Adding the configuration script line to ``~/.profile`` will help automatically
load it.

I rely on Sphinx for my notes and lab notes, so ought to set up a cron job to
build the documents.

Bah, turns out Python 3.5.0/1 is not supported due to missing ``Type`` in
the ``typing`` stdlib, and backports will also not work. Have to fix this
with another Python version somehow.
Had to do quite a bit of soul-searching to find
`this <https://github.com/PrefectHQ/prefect/issues/1247>`_.

Went ahead to install QPython 3.8.6 using the QNAP Club package, which comes
prepackaged with Sphinx and all the good stuff :)
Adding the python binaries to the PATH in ``~/.profile`` is then trivial:

.. sourcecode:: none

   export PATH=/opt/QPython3/bin:$PATH

Weirdly enough, the QPython3 binary disappeared (perhaps due to reboot?),
but I'll check up on it again.


QNAP Club
=========

App Center currently only contains official QNAP apps. Third-party app
repositories can be listed as well, especially the one by QNAP Club that
extends the QNAP, simply by adding their `repository link <https://www
.qnapclub.eu/en/repo.xml>`_ following the
instructions on their `QNAPClub installation page <https://
www.qnapclub.eu/en/howto/1>`_:

Apparently the CDN is currently experiencing a `major outage
<https://status.qnap.club/>`_ as at
21 December (hearsay is due to some server certificate issue on their end),
but nothing's stopping me from downloading the packages directly from the
link and uploading these files into the QNAP manually.

Other UNIX packages
-------------------

To install common UNIX packages, you need to first install
`Entware <https://github.com/Entware/Entware/wiki>`_, which is the updated
alternative to Optware. Not too familiar with it yet, to be honest.
Compiling and building from source is a big pain.

After installing the Entware package, the ``opkg`` binary is found in
``/opt/bin/opkg``, e.g. to install ``gcc``, call ``/opt/bin/opkg install gcc``
(of course, notwithstanding the other details mentioned in the
`Entware gcc setup page <https://github.com/Entware/Entware/wiki/
Using-GCC-for-native-compilation>`_).

I'm abandoning this route for the hopefully more straightforward pip
installation of ``mod-wsgi``. Seems like ``mod-wsgi`` is available directly
from ``ipkg``, but it is not listed in ``opkg``.

Update
------

This didn't work out the way I wanted either...

.. sourcecode:: none

   [~] # pip install mod-wsgi
   Collecting mod-wsgi
     Downloading mod_wsgi-4.7.1.tar.gz (498 kB)
       ERROR: Command errored out with exit status 1:
        command: /opt/QPython3/bin/python3.8 -c 'import sys, ... --egg-base /tmp/pip-pip-egg-info-gq5bjv1k
            cwd: /tmp/pip-install-86flxb45/mod-wsgi/
       Complete output (5 lines):
       Traceback (most recent call last):
         File "<string>", line 1, in <module>
         File "/tmp/pip-install-86flxb45/mod-wsgi/setup.py", line 165, in <module>
           raise RuntimeError('The %r command appears not to be installed or '
       RuntimeError: The 'apxs' command appears not to be installed or is not executable. Please check the list of prerequisites in the documentation for this package and install any missing Apache httpd server packages.
       ----------------------------------------
   ERROR: Command errored out with exit status 1: python setup.py egg_info Check the logs for full command output.

Suspect need to add ``apxs`` to PATH, while I have helpful discovered to be
``/usr/local/apache/bin/apxs``. Problem is, ``apxs`` is a Perl script as
stated in the shebang, but it doesn't even exist in the specified location...
``/usr/local/bin/perl`` does not exist.
This `dead QNAP thread <https://forum.qnap.com/viewtopic.php?t=59032>`_
also happens to mention this possible issue.

I'm now manually adding ``perl`` via ``opkg``, and doing a symlink:

.. sourcecode:: none

   ln -s /opt/bin/perl /usr/local/bin/perl

And presto...

.. sourcecode:: none

   [/usr/local/bin] # /usr/local/apache/bin/apxs
   perl: warning: Setting locale failed.
   perl: warning: Please check that your locale settings:
        LANGUAGE = (unset),
        LC_ALL = "en_US.UTF-8",
        LC_CTYPE = "en_US.UTF-8",
        LANG = "en_US.UTF-8"
   are supported and installed on your system.
   perl: warning: Falling back to the standard locale ("C").
   Can't locate strict.pm in @INC (you may need to install the strict module) (@INC contains: /opt/lib/perl5/5.28) at /usr/local/apache/bin/apxs line 19.
   BEGIN failed--compilation aborted at /usr/local/apache/bin/apxs line 19.

I'm giving up attempting ``mod_wsgi`` on QNAP, perhaps try running it on
a different Linux server from scratch instead.

Crontab
=======

.. sourcecode:: none

   # crontab -e
   */30 * * * * /etc/config/justin_web/build_sphinx.sh >/dev/null 2>&1

   # cat /etc/config/justin_web/build_sphinx.sh
   /opt/QPython3/bin/sphinx-build -M html /share/Web/notes/src /share/Web/notes/build

Note PATH is very restricted in cron. Check by redirecting output to file.

So turns out the crontab itself resets between reboots, according to
`this article <https://wiki.qnap.com/wiki/Add_items_to_crontab>`_.
To write cron jobs and make sure they survive the reboot:

1. Edit the crontab directly using ``vim /etc/config/crontab``
2. Reload using ``crontab /etc/config/crontab``
3. Restart the cron service ``/etc/init.d/crond.sh restart``
