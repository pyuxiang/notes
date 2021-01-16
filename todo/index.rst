===============================================================================
Todo
===============================================================================

.. raw:: html
    :file: timesheet.html

|

Module resources
================

🔍 CS3245 Information Retrieval
--------------------------------

- Website: https://www.comp.nus.edu.sg/~cs3245/
- Forum: `Piazza <https://piazza.com/class/kjmny91pkrx6ag>`_
- Tembusu Cluster: `Documentation <https://dochub.comp.nus.edu.sg/cf/guides/compute-cluster/start>`_

======================  ==========================================================
Deadline                Task
======================  ==========================================================
Every Friday, 12-14     Lecture via `Zoom <https://nus-sg.zoom.us/j/83413951734?pwd=YkdRbGZyT3czNmx5ZTlRVE9zK2hJQT09>`_
Every Friday, 9-10      Tutorial at COM1-0204, available online (upcoming)
30 April, 9-11          Final Examination
======================  ==========================================================

|

🔧 ESP4901 Research Project
----------------------------

- Website: https://pyuxiang.com/qoptics/

======================  ==========================================================
Deadline                Task
======================  ==========================================================
12 February             Submit intro and lit review of thesis: `folder <https://luminus.nus.edu.sg/modules/a8c230cb-9816-4aa9-b063-5ce0c86eb113/files/996934c9-214b-4210-a005-d096fd3c8f0f>`_
19 April                Final Report submission
11 May                  Panel Presentation
======================  ==========================================================

|

⚛️ CM3296 Molecular Modelling
------------------------------

======================  ==========================================================
Deadline                Task
======================  ==========================================================
Every week, twice       Lecture videos on `LumiNUS <https://luminus.nus.edu.sg/modules/565aee51-c2e7-4e91-89d4-34e572afb980/files/72af26f3-5824-4df9-90b7-caaa7e1fe59c>`_
Every Wednesday, 17-18  Consultation via `Zoom <https://teams.microsoft.com/l/team/19%3a7c30a3d3a93946858156ca0be271edaf%40thread.tacv2/conversations?groupId=50da3a4f-cb61-4773-a0b4-0c09484ab62b&tenantId=5ba5ef5e-3109-4e77-85bd-cfeb0d347e82>`_
======================  ==========================================================

|

⌨️ CS2103 Software Engineering
-------------------------------

- Website: http://www.comp.nus.edu.sg/~cs2103
- Forum: `GitHub Issues <https://github.com/nus-cs2103-AY2021S2/forum/issues>`_
- Peer evaluation: `TeamMATES <https://teammatesv4.appspot.com/web/front/home>`_
- Other chat channels: `Gitter <https://gitter.im/nus-cs2103-AY2021S2/community>`_ / `Teams <https://teams.microsoft.com/l/team/19%3a6ac468d3601d448a89069f389d42d6f7%40thread.tacv2/conversations?groupId=29cec2b3-fc09-4b75-b6f1-faabdf41dbf4&tenantId=5ba5ef5e-3109-4e77-85bd-cfeb0d347e82>`_
- Textbook: `PDF <modules/CS2103_textbook.pdf>`_ / `Web <https://nus-cs2103-ay2021s2.github.io/website/se-book-adapted/index.html>`_

======================  ==========================================================
Deadline                Task
======================  ==========================================================
Every Friday 14-16      Lecture on `Zoom <https://nus-sg.zoom.us/j/84606514077?pwd=TTZ5ZXF5ZGVMZUlBTzJLa01teDBxdz09>`_
Every Wednesday, 16-17  Tutorial, online
Every week              Weekly Quiz via `LumiNUS <https://luminus.nus.edu.sg/modules/0205ba47-730b-4cf2-a48d-fbddcd3553d0/quiz>`_
24 April, 9-11          Final Examination
======================  ==========================================================

|

💡 QT5201U Quantum control technology
--------------------------------------

- Website: https://qt5201.org/
- Forum: `LumiNUS <https://luminus.nus.edu.sg/modules/6c5aa190-efd5-4777-a868-8ea1731c01cd/forum>`_

======================  ==========================================================
Deadline                Task
======================  ==========================================================
Every Monday 16-18      Lecture in CQT/SR0315
Every Tuesday 14-16     Lab work in CQT/SR0315
======================  ==========================================================

|

💾 EE5440 Magnetic Data Storage for Big Data
---------------------------------------------

- Lecture notes: `LumiNUS <https://luminus.nus.edu.sg/modules/6a49fd16-a11f-4fce-b2e5-04ea2af7293e/files>`_

======================  ==========================================================
Deadline                Task
======================  ==========================================================
Every Tuesday 18-21     Lecture in E1-06-03, recorded on `Panopto <https://mediaweb.ap.panopto.com/Panopto/Pages/Sessions/List.aspx?embedded=0#folderID=%225b224ea6-3cf8-4696-80c4-acae00dbdf38%22>`_
28 Apr, 13-15           Final Examination
======================  ==========================================================

|

Others
======

- Finish up resume
- Job applications (SDI, S15)
- GDS toolbox (pkg)
- GNUplot for live web graphs (ChartJS?)
- Embed Google timesheet
- Unexplored links:

    - https://jekyllrb.com/
    - https://www.fullstackpython.com/web-development.html
    - https://wiki.python.org/moin/WebFrameworks
    - https://jinja.palletsprojects.com/en/2.11.x/
    - https://www.business2community.com/strategy/hierarchy-vs-flat-task-lists-part-1-01690331
    - https://www.business2community.com/strategy/hierarchy-vs-flat-task-lists-part-2-01705009
    - https://en.wikipedia.org/wiki/Nelder%E2%80%93Mead_method
    - https://github.com/kurtsiefer/linuxmeasurement

- Migrate subdomains to paths under main website

    - Explored certbot, but seems like will require compatible nameserver
    - LEgo seems to be available... curl cannot use cos libnettle.so.6 missing

After much reading and whatnot, decided to pass on the whole SSL wildcard
authentication thing, make my life simpler, and shift all the subdomain
pages into the same domain but in subdirectory instead, i.e.
``notes.pyuxiang.com`` goes to ``pyuxiang.com/notes`` instead. Will have
different styles, but at least they're still independent.

Managed to segregate the directories nicely too.

|
