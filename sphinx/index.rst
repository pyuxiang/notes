===============================================================================
Sphinx
===============================================================================

Use ``sphinx-autobuild`` to automate building. Would like to use a service
for next time.

.. code-block::

    sphinx-autobuild --port=0 --open-browser src build/html

Problem is not sure which direction to take?

- Facebook's ``watchman`` documentation is hard to decipher
- Window's ``schtasks.exe`` opens a command prompt with every task
- NSSM is an added dependency...?

Other alternatives using pip ``watchdog``...
