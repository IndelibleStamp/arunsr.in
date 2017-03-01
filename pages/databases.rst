.. title: databases
.. slug: databases
.. date: 2017-03-01 10:41:56 UTC+05:30
.. tags: linux, notes, db
.. category: linux
.. link: 
.. description: 
.. type: text

.. contents::


Postgreql quickstart
--------------------

.. code:: Sql

          sudo -i -u postgres postgresql quickstart
          psql >createuser --interactive createdb ttrssdb
          psql >alter user ttrssuser with encrypted password 'blah';
          psql >grant all privileges on database ttrssdb to ttrssuser;


MySql quick start
-----------------

.. code:: Sql

    mysql> create database habari;
    Query OK, 1 row affected (0.02 sec)

    mysql> grant all on habari.* to 'habariuser'@'localhost' identified by 'blah';
    Query OK, 0 rows affected (0.06 sec)

    mysql> flush privileges;
    Query OK, 0 rows affected (0.00 sec)

Sqlite basics:
--------------

.. code:: Sql

        thaum ~/code/app$ sqlite perl.db
        SQLite version 2.8.17
        Enter ".help" for instructions
        sqlite> .tables
        sqlite> .schema
        sqlite> create table perltest (id integer PRIMARY KEY,name varchar(10), salary integer);
        sqlite> .tables
        perltest
        sqlite> .headers on
        sqlite> .mode column
        sqlite> select * from perltest;
        sqlite> insert into perltest values(1,'arun',12345);
        sqlite> insert into perltest values(2,'brun',23456);
        sqlite> select * from perltest;
        id          name        salary
        ----------  ----------  ----------
        1           arun        12345
        2           brun        23456

