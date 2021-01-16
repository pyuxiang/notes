==============================================================================
Database
==============================================================================

Some quick notes for MariaDB on the QNAP.
Meant to be direct port of MySQL (but both are open-source...?)

Dump of everything I was working on today and yesterday.
Still trying to figure out how to do RESTful APIs.

Setting up

- Search for the binary using ``find / -name mysql``
- QNAP has aliases for the same directory:
  ``/usr/local/mysql`` and ``/usr/local/mariadb``
- Connect to the database using ``/usr/local/mariadb/bin/mysql -u root -p``

.. sourcecode::

    Welcome to the MariaDB monitor.  Commands end with ; or \g.
    Your MariaDB connection id is 493
    Server version: 5.5.57-MariaDB MariaDB Server

    Copyright (c) 2000, 2017, Oracle, MariaDB Corporation Ab and others.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

    MariaDB [(none)]> USE test;
    Database changed
    MariaDB [test]> SELECT * from test_table;
    +----+--------+------------+---------+
    | id | name   | date       | workout |
    +----+--------+------------+---------+
    |  1 | justin | 0000-00-00 |       1 |
    |  2 | justin | 0000-00-00 |       1 |
    +----+--------+------------+---------+
    2 rows in set (0.00 sec)

MariaDB

- root/admin default password
- QNAP-specific: https://forum.qnap.com/viewtopic.php?t=138714
- phpMyAdmin gives direct access to database: https://pyuxiang.com/phpMyAdmin
- Good tutorial and background on databases: https://mariadb.com/kb/en/database-design-phase-2-conceptual-design/
- Essentially data structure choice for indices: https://www.sqlshack.com/what-is-the-difference-between-clustered-and-non-clustered-indexes-in-sql-server/

SQL server

- Do not expose SQL server to web:
  `reddit <https://www.reddit.com/r/sysadmin/comments/709te7/best_practice_for_exposing_sql_server_to_internet/>`_,
  `sqlserverscience <https://www.sqlserverscience.com/security/internet-access-to-your-sql-server/>`_,
  `so <https://stackoverflow.com/questions/52887612/connecting-to-phpmyadmin-using-php-on-a-qnap-nas>`_
- Standard practice to use SSH or VPN as gateway
- Equifax breach: https://www.csoonline.com/article/3444488/equifax-data-breach-faq-what-happened-who-was-affected-what-was-the-impact.html

Sending and processing POST requests

- ``mod_python`` is inactive, while ``mod_wsgi`` is currently uncompilable
  on the QNAP. Need to look for alternative scripting languages.
- PHP seems appropriate:
  `insert into SQL <https://www.w3schools.com/php/php_mysql_insert.asp>`_
- HTML5 form examples: `one <https://www.cloudways.com/blog/custom-php-mysql-contact-form/#prereq>`_,
  `two <https://www.w3schools.com/html/html_forms.asp>`_,
  `three <https://www.w3schools.com/php/php_form_complete.asp>`_

LAMP looks like the minimalist way to go.

Graph plotting

- Can't seem to build matplotlib in the QNAP, resorting to GNUplot for
  server-side plotting of the gym graphs.
- `GNUplot: Basic setup <https://unix.stackexchange.com/questions/392213/how-to-plot-graph-from-a-text-file-values-using-gnuplot>`_
- `GNUplot: Multicolor <https://stackoverflow.com/questions/22037858/gnuplot-change-color-of-the-connecting-lines>`_
- `GNUplot: Multiple graphs <https://stackoverflow.com/questions/11092608/gnuplot-plotting-data-from-multiple-input-files-in-a-single-graph>`_
- `gplot.py: Standalone Python interface for GNUplot <https://github.com/mzechmeister/python/wiki/gplot.py>`_
