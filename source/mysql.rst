MySQL
=====

There are many versions of MySQL available via Homebrew, and they are mostly
compatible with each other and with our production server. This document
describes installing MySQL version 5.7, because that's what we have elsewhere.
If that version doesn't work, feel free to try another version.

Installation
------------

.. code-block:: shell
  :linenos:

  $ brew install mysql@5.7
  $ brew link mysql@5.7 --force
  $ brew services start mysql@5.7

.. note::

  ``@5.7`` specifies which version of MySQL to install. If you leave that part out
  the command will install the most recent version, which may lead to problems
  later.

Line 1 above installs MySQL.

Line 2 tells brew to make sure that the mysql
commands are linked and available at the command line. If we hadn't included
a version number in line 1, line 2 wouldn't be necessary.

Line 3 enables MySQL as a background service and registers it to start automatically
at login.

Now it's time to test the installation.

.. code-block:: shell

  $ mysql -u root

This should start a MySQL shell in the terminal that looks something like this::

  Welcome to the MySQL monitor.  Commands end with ; or \g.
  Your MySQL connection id is 4
  Server version: 5.7.22 Homebrew

  Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.

  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
  mysql>

The ``mysql>`` line is the command prompt. MySQL is waiting for a command at that
prompt. Type ``ctrl-d`` [#f1]_ to exit the MySQL shell and return to your normal shell.

Make it secure
--------------

MySQL is installed without a password and with only a root user. It must be made
secure.

.. code-block:: shell

  $ mysql_secure_installation

The ``mysql_secure_installation`` command will ask a few questions about
configuring the MySQL server. Recommended settings include:

 * **Do not** install the Validate Password Plugin.
 * **Do** set a memorable password for the root account. Use a password that is
   different from any other password you use to login.
 * **Do** remote the anonymous users.
 * **Do not** allow remote users to login [#f2]_.
 * **Do** remove the test database and access to it.
 * **Do** reload the privilege tables.

With these settings, only a user logged in to your computer can access the
database, and must provide a password you assign to do so.

Make it easier to use
---------------------

MySQL's tools will look in your home directory for a file called ``.my.cnf``
which may contain additional settings and configuration. You can use this file
to automatically supply a username and password when connecting to the server.

Use the editor of your choice to edit the file. It is divided up in to sections,
with one section for each mysql command you configure. Here are some suggested
contents with descriptions

.. code-block:: ini
  :linenos:

  [mysql]
  prompt=mysql \d >
  user=root
  password=abc123

Line 1 starts the ``mysql`` section. The options in this section will apply to
that command only.

Line 2 adds the current database name to the command prompt in the MySQL shell.

Lines 3 and 4 set the connection parameters so you can connect without passing
the username and password at the command line.

Use MySQL
---------

The database is fairly easy to use from the command line if you've configured it
as above. Type ``mysql`` and get a prompt. Type ``mysql name`` to connect to
the named database.

There are other tools to connect to MySQL. `MySQLWorkBench`_ provides a nice GUI
to browse databases and tables, and can perform useful management operations on
them. `Netbeans`_ provides a useful GUI as well, although it is a bit less
sophisticated. It's also a very powerful PHP editor.

.. _MySQLWorkBench: https://www.mysql.com/products/workbench/
.. _Netbeans: https://netbeans.org/projects/www/

Each project that uses MySQL has installation instructions specific to that
project. The documentation will guide you through creating a database, adding
a database-specific user, setting permissions for that user, and setting a
password for that user.

.. todo::

  Include a mention of ``mysqldump`` for creating backups and how to load those
  backups back into MySQL.

.. rubric:: Footnotes

.. [#f1] ``ctrl-d`` means to hold down the control key and type d.
.. [#f2] The wording of the question in the command is a bit odd: "Disallow root
  login remotely?" Answer **y** to this question.