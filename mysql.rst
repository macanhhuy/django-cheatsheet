MySQL - General tips
====================

Ubuntu: need to install (to use MySQL with Django)::

   sudo apt-get install mysql-client mysql-server libmysqlclient-dev

Django::

   pip install mysqlclient

   DATABASES['default'] = {
      'ENGINE': 'django.db.backends.mysql',
      'NAME': 'dbname',
      'USER': 'username',
   }


General MySQL tips
------------------
(These are not Django-specific.)

Starting the client::

    $ mysql --user=username [database]      # if user has no password
    $ mysql --user=username --password [database]   # to be prompted for password

In the client::

    mysql> CREATE USER 'username' IDENTIFIED BY 'plaintextpassword';       # Create user with password
    mysql> CREATE USER 'username'@'localhost';   # no password, can only connect locally
    mysql> SHOW DATABASES;
    mysql> CREATE DATABASE databasename;
    mysql> GRANT ALL PRIVILEGES ON databasename.* TO "username"@"hostname" IDENTIFIED BY "password";
    mysql> FLUSH PRIVILEGES;
    mysql> DROP DATABASE databasename;
    mysql> DROP USER username;
    mysql> EXIT
    Bye

Change user password
~~~~~~~~~~~~~~~~~~~~

Note: default host is '%' which will not let you connect via unix socket, must set password for host 'localhost' to allow that::

    mysql> update mysql.user set password=password('foo'),host='localhost' where user='poirier_wordpres';
    mysql> flush privileges;

Recover lost password
~~~~~~~~~~~~~~~~~~~~~

http://dev.mysql.com/doc/refman/5.5/en/resetting-permissions.html

C.5.4.1.3. Resetting the Root Password: Generic Instructions
On any platform, you can set the new password using the mysql client::

    Stop mysqld
    Restart it with the --skip-grant-tables option. This enables anyone to connect without a password and with all privileges. Because this is insecure, you might want to use --skip-grant-tables in conjunction with --skip-networking to prevent remote clients from connecting.

    $ mysql
    mysql> UPDATE mysql.user SET Password=PASSWORD('MyNewPass') WHERE User='root';
    mysql> FLUSH PRIVILEGES;
    mysql> EXIT

    Stop the server
    Restart it normally (without the --skip-grant-tables and --skip-networking options).

Make a dump::

    mysqldump _dbname_ > dumpfile.sql
    mysqldump --result-file=dumpfile.sql _dbname_

Restore a dump::

    mysql dbname < dumpfile.sql

Create a new MySQL database
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Step by step::

    $ mysql -u root -p
    <ENTER MYSQL ROOT PASSWORD>
    mysql> create user 'ctsv2_TR'@'localhost';
    mysql> create database ctsv2_TR;
    mysql> grant all privileges on ctsv2_TR.* to 'cstv2_TR'@'localhost';
    mysql> flush privileges;
    mysql> exit
    Bye

Restore data into database
~~~~~~~~~~~~~~~~~~~~~~~~~~

This is just a matter of piping the dump into the mysql client::

    gunzip <ctsv2_TR_20140602.sql.gz | mysql -u ctsv2_TR ctsv2_TR

