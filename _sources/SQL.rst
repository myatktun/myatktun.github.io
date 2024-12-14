===
SQL
===

1. `What is SQL?`_
2. `Data Types`_
3. `Commands & Queries`_
4. `ER Diagrams`_

`back to top <#sql>`_

What is SQL?
============

* `RDBMS Basics`_
* Structured Query Language, used to communicate with data in RDBMS (Relational Database
  Management System)

RDBMS Basics
------------
    * **Column**: define single attribute
    * **Row**: define single entry
    * **Primary Key**: special attribute to make a row unique, each table needs primary key
      column
    * **Surrogate Key**: primary key that serves only as unique id, no mapping to the
      real-world data
    * **Natural Key**: primary key that has a mapping to the real-world data
    * **Foreign Key**: primary key in another table and relates to it, a table can have
      more than one foreign key, need to create table first before defining foreign keys
    * **Composite Key**: made up of two keys (primary or foreign), used when one column
      doesn't uniquely identify each row

* different RDBMS implement SQL differently
* SQL code is not always portable to another without modification
* hybrid language, combination of 4
    * DQL (Data Query Language)
        - to query already present data
    * DDL (Data Definition Language)
        - to define database schemas
    * DCL (Data Control Language)
        - to control access to the data, user and permission management
    * DML (Data Manipulation Language)
        - to insert, update and delete data

`back to top <#sql>`_

Data Types
==========

* some data types differ depending on DBMS

INT
---
    * integer, whole numbers
    * e.g 1, 10, 999

DECIMAL(M,N)
------------
    * M: total number of digits
    * N: total number of digits after the decimal point
    * e.g DECIMAL(7,3): 1234.567

VARCHAR(l)
----------
    * variable character, string
    * l: maximum number of characters
    * e.g VARCHAR(3): abc

BLOB
----
    * binary large object, usually for images or files

DATE
----
    * specific format of date
    * e.g 'YYYY-MM-DD'

TIMESTAMP
---------
    * specific date format with time, usually for records
    * e.g 'YYYY-MM-DD HH:MM:SS'

`back to top <#sql>`_

Commands & Queries
==================

* instructions to manipulate and fetch the database
* convention that SQL commands are in upper case

create DB
---------
    * ``CREATE DATABASE db_name;``

list DBs
--------
    * ``SHOW DATABASES;``

switch DB
---------
    * ``USE db_name;``

create table
------------
    * ``CREATE TABLE table_name (column_1 INT PRIMARY KEY, column_2 VARCHAR(50));``
    * ``CREATE TABLE t1 (c1 INT, c2 VARCHAR(50), PRIMARY KEY(c1));``
    * ``CREATE TABLE t1(PRIMARY KEY(c1,c2));`` set multiple attributes as primary, composite key
    * ``CREATE TABLE t1 (c1 INT NOT NULL);`` (c1 cannot be empty when insert)
    * ``CREATE TABLE t1 (c1 INT UNIQUE);`` (cannot insert duplicate values)
    * primary key is ``NOT NULL`` and ``UNIQUE`` by default
    * ``CREATE TABLE t3(FOREIGN KEY(c1) REFERENCES t2(c1) ON DELETE SET NULL);`` add foreign key
      while creating table, referenced table must already exist
    * ``CREATE TABLE t3(FOREIGN KEY(c1) REFERENCES t2(c1) ON DELETE CASCADE);``
    * some foreign keys must be set to ``NULL`` first, and then updates it, as the other table
      isn't created or populated yet

list tables
-----------
    * ``SHOW TABLES;``

show structure of the table
---------------------------
    * ``DESCRIBE t1;``

delete table
------------
    * ``DROP TABLE t1;``

Constraints
-----------
    * ``NOT NULL``, ``UNIQUE``
    * ``DEFAULT``: ``CREATE TABLE t1 (c1 INT DEFAULT 66);``
    * ``AUTO_INCREMENT``: data can be left when insert, will auto increase based on previous,
      default start at 1, use `ALTER TABLE t1 AUTO_INCREMENT=100;` to change
    * ``ON DELETE SET NULL``: foreign key will be set to ``NULL`` if row on referenced table is
      deleted
    * ``CREATE TABLE t3(FOREIGN KEY(c1) REFERENCES t2(c1) ON DELETE SET NULL);`` t3.c1 will be
      "sql" resulted in an error at token: '$'`NULL` if t2.c1 is deleted
    * ``ON DELETE CASCADE``: row on foreign key will be deleted if row on referenced table is
      deleted
    * use when foreign key will also be primary key of the table
    * ``CREATE TABLE t3(FOREIGN KEY(c1) REFERENCES t2(c1) ON DELETE CASCADE);`` t3.c1 will be
      deleted if t2.c1 is deleted

modify column
-------------
    * ``ALTER TABLE t1 ADD c3 DECIMAL;``
    * ``ALTER TABLE t1 DROP COLUMN c3;``
    * ``ALTER TABLE t1 ADD FOREIGN KEY(c1) REFERENCES t2(c1);`` add foreign key to existing table

insert data
-----------
    * ``INSERT INTO t1 VALUES(x, y);`` (order matters)
    * ``INSERT INTO t1(c1) VALUES(x);`` (insert only specific attribute)
    * cannot insert values with duplicate primary key

update data
-----------
    * ``UPDATE t1 SET c1='new_value' WHERE c1='old_value;'``
    * ``UPDATE t1 SET c1='new_value' WHERE c1='x' OR c1='y;'``
    * ``UPDATE t1 SET c1='new_value',c2='new_value' WHERE c1='old_value;'``
    * ``UPDATE t1 SET c1='new_value';'`` (apply to every row)

delete data
-----------
    * ``DELETE FROM t1;`` (delete every row)
    * ``DELETE FROM t1 WHERE c1='x';``

get data
--------
    * ``SELECT * FROM t1;``
    * comparison operators
        - ``<, >, <=, >=, =, <> (not equal), AND, OR``
    * ``-- this is a comment``
    * ``SELECT c1 FROM t1;`` or ``SELECT t1.c1 FROM t1;`` (prefix with table_name is useful for
      complex queries)
    * ``SELECT c1,c2 FROM t1 ORDER BY c2;`` (ASC by default)
    * ``SELECT c1,c2 FROM t1 ORDER BY c2 DESC;``
    * ``SELECT c1,c2 FROM t1 ORDER BY c3;`` (can order even if not included in query)
    * ``SELECT c1,c2 FROM t1 ORDER BY c2,c1;`` (order by c2 first, order by c1 again if needed)
    * ``SELECT * FROM t1 LIMIT 3;`` (limit result row numbers)
    * ``SELECT c1 FROM t1 WHERE c1='x' OR c3='y';`` (filter)
    * ``SELECT * FROM t1 WHERE c1 IN ('v1', 'v2', 'v3');`` (can be any one of the values)
    * ``SELECT c1 AS new_name FROM t1;`` change result column name
    * ``SELECT DISTINCT c1 FROM t1;`` get only distinct attributes

functions
---------
    * special block of code used when querying data
    * count
        - ``SELECT COUNT(c1) FROM t1;``
        - ``SELECT COUNT(c1) FROM t1 WHERE c1='x' AND c2>'y';``
    * average
        - ``SELECT AVG(c1) FROM t1;``
    * sum
        - ``SELECT SUM(c1) FROM t1;``
    * aggregate
        - ``SELECT COUNT(c1),c2 FROM t1 GROUP BY c2;``

wildcards
---------
    * ``%``: any number of characters, ``_``: one character
    * ``SELECT * FROM t1 LIKE '%og';`` e.g 'aog' or 'aaaaog'
    * ``SELECT * FROM t1 LIKE '%og%';`` e.g 'aaaaogaaaa'
    * ``SELECT * FROM t1 LIKE '__og';`` e.g 'aaog'

union
-----
    * combine columns from ``SELECT`` statements
    * ``SELECT c1 FROM t1 UNION SELECT c2 FROM t2;`` result c1 and c2 in single column
    * number of columns must be same to union, e.g cannot be ``c1,c2 FROM t1 UNION c2 FROM t2``
    * columns must be of same or compatible data type to union, different data types will be
      converted if possible
    * ``SELECT c1 as new_name FROM t1 UNION SELECT c2 FROM t2;`` result column will be ``new_name``

join
----
    * join separate tables on specific common column
    * **inner join**
        - only common will be returned
        - ``SELECT t1.c1, t1.c2, t2.c2 FROM t1 JOIN t2 ON t1.c1 = t2.c1;`` return only those
          where ``t1.c1 = t2.c1``
    * **left join**
        - return all from ``t1`` even if not common
        - ``SELECT t1.c1, t1.c2, t2.c2 FROM t1 JOIN t2 ON t1.c1 = t2.c1;`` return not only those
          where ``t1.c1 = t2.c1`` but also others from ``t1``
    * **right join**
        - return all from ``t2`` even if not common
        - ``SELECT t1.c1, t1.c2, t2.c2 FROM t1 JOIN t2 ON t1.c1 = t2.c1;`` return not only those
          where ``t1.c1 = t2.c1`` but also others from ``t2``
    * **full outer join**
        - left join and right join combined
        - cannot do in MySQL

nested queries
--------------
    * using results from one query in another
    * ``SELECT t1.c1 FROM t1 WHERE t1.c2 IN (SELECT t2.c1 FROM t2 WHERE t2.c1 = x);`` execute the
      query in the () first and use that result to execute outer query
    * use constraints if necessary, as result from one query could be incompatible with other,
      especially when using equal

triggers
--------
    * block of SQL that will be performed when certain action is operated
    * e.g add a row to t2 when certain data is deleted on t1
    * create trigger

        .. code-block:: sql

           DELIMITER $$ -- change SQL delimiter from ';' to '$$'
           CREATE
               TRIGGER trig1 BEFORE INSERT -- can also use AFTER INSERT/DELETE/UPDATE
               ON t1
               FOR EACH ROW BEGIN
                   INSERT INTO t2 VALUES('action triggered');
                   INSERT INTO t2 VALUES(NEW.c1); -- add value of c1, to be added to t1, into t2 first
                   IF NEW.c1 = 'x' THEN
                       INSERT INTO t2 VALUES(NEW.c1);
                   ELSEIF NEW.c1 = 'y' THEN
                       INSERT INTO t2 VALUES('wrong value');
                   ELSE
                       INSERT INTO t2 VALUES('default value');
                   END IF;
               END$$ -- CREATE command ends here
           DELIMITER ; -- change SQL delimiter back to ';'


    * delete trigger: ``DROP TRIGGER trig1;``

`back to top <#sql>`_

ER Diagrams
===========

* `Entity`_, `Attributes`_, `Relationship`_, `Convert to Schema`_
* Entity Relationship Diagrams, helps to convert storage/business requirements into database
  schema
* uses shapes and arrows or graphs to define relationship

Entity
------
    * object to be modeled and stored information about
    * use rectangle shape
    * can define more than one entity in a diagram
    * **weak entity**
        - cannot be uniquely identified only by its attributes
        - usually depends on other entities
        - use double-layered rectangle shape
        - always need to have total participation in the relationship

Attributes
----------
    * specific information about an entity
    * use oval shape
    * **primary key**
        - uniquely define an entry in the database table
        - use oval shape, text is underlined
    * **composite attribute**
        - can be broken into sub-attributes
        - e.g name can be separated into first_name and last_name
    * **multi-valued attribute**
        - can have more than one value
        - use double-layered oval shape
        - e.g a person can have more than one phone numbers
    * **derived attribute**
        - can be derived from other attributes
        - use dotted-line oval shape
        - e.g a student's attribute ``passed`` can be derived from attribute ``test_score``

Relationship
------------
    * connect multiple entities in a single diagram
    * use diamond shape
    * connect with single line for partial participation and double line for total
    * **relationship attribute**
        - a relationship can have attributes
        - attributes are not stored on entity but on the relationship
    * **relationship cardinality**
        - maximum number of times an instance in one entity can relate to instances of another
          entity
        - e.g 1:1, 1:N, N:M
    * **identifying relationship**
        - use double-layered diamond shape
        - serves to uniquely identify the weak entity

Convert to Schema
-----------------
    * create relation/table for each regular entity with all simple attributes
    * create relation/table for each weak entity with all simple attributes
        - primary key should be the key of weak entity plus the primary key of its owner
    * map binary (two entity participating) 1:1 relationship types
        - add primary key of one entity as foreign key in the one that has total participation
        - if both are partial or total, use most convenient approach
    * map binary 1:N relationship types
        - add ``1`` side primary key as foreign key on the ``N`` side
    * map binary M:N relationship types
        - create new relation/table, with primary key as the combination of both entities'
          primary keys, and include any relationship attributes

`back to top <#sql>`_
