===============================================================================
Log
===============================================================================

2020-12-21
==========

Have been trying to figure out how to deal with webpages being cached by the
browser, causing my webpages to not be updated at all.

Fiddling with ``mod_expires`` by adding the line
``ExpiresDefault "access plus 2 minutes"`` into the Apache configuration file
caused the whole QNAP to bug out. And I didn't save any backups...
Spent the next two hours trying to get the settings back.
