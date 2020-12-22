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

Crontab
=======

.. sourcecode:: none

   # crontab -e
   */30 * * * * /etc/config/justin_web/build_sphinx.sh >/dev/null 2>&1

   # cat /etc/config/justin_web/build_sphinx.sh
   /opt/QPython3/bin/sphinx-build -M html /share/Web/notes/src /share/Web/notes/build

Note PATH is very restricted in cron. Check by redirecting output to file.
