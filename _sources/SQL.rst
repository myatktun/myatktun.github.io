===
SQL
===

1. `Basics`_
2. `Data Types`_
3. `Commands & Queries`_
4. `ER Diagrams`_
5. `References & External Resources`_

`back to top <#sql>`_

Basics
======

* `RDBMS Basics`_, `SQL Categories`_
* Structured Query Language, used to communicate with data in RDBMS (Relational Database
  Management System)


RDBMS Basics
------------
    * **Table**
        - structured list of specific data type
        - table names are unique and cannot use the same name in the same database, but can be
          reused in different ones
        - some databases use the owner name as part of the unique table name
        - Schema: describe database and table layout and properties
    * **Column**
        - define single attribute in a table, has an associated data type
        - tables are made up of one or more columns
        - data should broken up into different columns
        - combined data in one column makes it difficult to sort or filter
    * **Row**
        - a single entry/record in a table
    * **Primary Key**
        - a column or a set of columns to make a row unique, used to refer to a specific row
        - not mandatory, but each table should have a primary key column
        - cannot have duplicates, be ``NULL``, be modified, and can never be reused even if a row
          is deleted
    * Surrogate Key: primary key that serves only as unique id, no mapping to the
      real-world data
    * Natural Key: primary key that has a mapping to the real-world data
    * Foreign Key: primary key in another table and relates to it, a table can have
      more than one foreign key, need to create table first before defining foreign keys
    * Composite Key: made up of two keys (primary or foreign), used when one column
      doesn't uniquely identify each row
    * different RDBMS implement SQL differently

SQL Categories
--------------
    * SQL code is not always portable to another without modification
    * DQL (Data Query Language): to query already present data
    * DDL (Data Definition Language): to define database schemas
    * DCL (Data Control Language): control access to the data, user and permission management
    * DML (Data Manipulation Language): to insert, update and delete data
    * TCL (Transaction Control Language): manage transactions within the database

`back to top <#sql>`_

Data Types
==========

* `Numeric`_, `String`_, `Binary`_, `Date & Time`_
* restrict the type of data to be stored in a column
* some data types differ depending on DBMS


Numeric
-------
    * ``INT``
        - integer, whole numbers
        - e.g 1, 10, 999
    * ``DECIMAL(M,N)``
        - M: total number of digits
        - N: total number of digits after the decimal point
        - e.g DECIMAL(7,3): 1234.567

String
------
    * ``VARCHAR(l)``
        - variable character, string
        - l: maximum number of characters
        - e.g VARCHAR(3): abc

Binary
------
    * ``BLOB``
        - binary large object, usually for images or files

Date & Time
-----------
    * ``DATE``
        - specific format of date
        - e.g 'YYYY-MM-DD'
    * ``TIMESTAMP``
        - specific date format with time, usually for records
        - e.g 'YYYY-MM-DD HH:MM:SS'

`back to top <#sql>`_

Commands & Queries
==================

* `DDL`_, `DML`_, `DQL`_, `DCL`_, `TCL`_, `Constraints`_, `Functions`_, `Nested Queries`_, `Joins`_, `Union`_, `Triggers`_
* instructions to manipulate and fetch the database, upper case convention
* comment: ``-- single line``, ``/* multi line */``
* operators: ``=, !=, <> (not equal), <, <=, !<, >, >=, !>, AND, OR, BETWEEN, IS NULL, NOT, IN``


DDL
---
    * ``CREATE``
        - primary key is ``NOT NULL`` and ``UNIQUE`` by default
        - some foreign keys must be set to ``NULL`` first, and then updates it, as the other table
          isn't created or populated yet

        .. code-block:: sql

           CREATE DATABASE db_name;
           CREATE TABLE table_name (column_1 INT PRIMARY KEY, column_2 VARCHAR(50));
           CREATE TABLE t1 (c1 INT, c2 VARCHAR(50), PRIMARY KEY(c1));
   
           -- set multiple attributes as primary, composite key
           CREATE TABLE t1(PRIMARY KEY(c1,c2));
   
           CREATE TABLE t1 (c1 INT NOT NULL); -- c1 cannot be empty when insert
           CREATE TABLE t1 (c1 INT UNIQUE); -- cannot insert duplicate values
   
           -- add foreign key while creating table, referenced table must already exist
           CREATE TABLE t3(FOREIGN KEY(c1) REFERENCES t2(c1) ON DELETE SET NULL);
           CREATE TABLE t3(FOREIGN KEY(c1) REFERENCES t2(c1) ON DELETE CASCADE);


    * ``ALTER``
        - modify column

        .. code-block:: sql

           ALTER TABLE t1 ADD c3 DECIMAL;
           ALTER TABLE t1 DROP COLUMN c3;
           ALTER TABLE t1 ADD FOREIGN KEY(c1) REFERENCES t2(c1); -- add FK to existing table


    * ``DROP``

        .. code-block:: sql

           DROP TABLE t1;


    * ``TRUNCATE``
    * ``USE``

        .. code-block:: sql

           USE db_name;


    * ``SHOW``
        - non-standardised but widely used to get information about database environment and
          structure

        .. code-block:: sql

           SHOW DATABASES; -- list databases
           SHOW TABLES; -- list tables



DML
---
    * ``INSERT``
        - cannot insert values with duplicate primary key

        .. code-block:: sql

           INSERT INTO t1 VALUES(x, y); -- order matters
           INSERT INTO t1(c1) VALUES(x); -- insert only specific attribute


    * ``UPDATE``

        .. code-block:: sql

           UPDATE t1 SET c1='new_value' WHERE c1='old_value';
           UPDATE t1 SET c1='new_value' WHERE c1='x' OR c1='y';
           UPDATE t1 SET c1='new_value',c2='new_value' WHERE c1='old_value';
           UPDATE t1 SET c1='new_value'; -- apply to every row


    * ``DELETE``

        .. code-block:: sql

           DELETE FROM t1; -- delete every row
           DELETE FROM t1 WHERE c1='x';



DQL
---
    * ``SELECT``, ``DESCRIBE``
    * different DBMSs and clients display data differently
    * data formatting is a presentation issue, not a retrieval issue
    * avoid wildcards if possible, as they can reduce the performance of retrieval
    * Clause Ordering: ``SELECT``, ``FROM``, ``WHERE`` or ``GROUP BY``, ``HAVING`` or ``ORDER BY``
    * **Retrieve**
        - the first row retrieved is row 0

        .. code-block:: sql

           SELECT * FROM t1; -- retrieve all columns
   
           SELECT c1 FROM t1;
           SELECT t1.c1 FROM t1; -- prefix with table_name is useful for complex queries
   
           -- get only distinct attributes, multiple columns will be combined and get unique ones
           SELECT DISTINCT c1 FROM t1;
   
           SELECT c1 FROM t1 LIMIT 3; -- limit result row numbers
           SELECT c1 FROM t1 LIMIT 3 OFFSET 3; -- get limited rows starting from the offset
           SELECT c1 AS new_name FROM t1; -- change result column name


    * **Sort**
        - ASC by default, should be after ``WHERE`` clause
        - letter cases are same in dictionary sort order, but can be changed by DBMS admin
        - sorts generated output

        .. code-block:: sql

           SELECT c1,c2 FROM t1 ORDER BY c2;
           SELECT c1,c2 FROM t1 ORDER BY c1, c2 DESC; -- descending only by c2
           SELECT c1,c2 FROM t1 ORDER BY c3; -- can order even if not included in query
           SELECT c1,c2 FROM t1 ORDER BY c2,c1; -- order by c2 first, order by c1 again if needed
   
           -- order by relative column position in the SELECT list
           SELECT c1, c2, c3 FROM t1 ORDER BY 2, 3

    * **Filter**
        - ``AND`` is processed before ``OR``

        .. code-block:: sql

           SELECT c1 FROM t1 WHERE c1='x' ORDER BY c2; -- filter and sort
           SELECT c1 FROM t1 WHERE c1 BETWEEN x AND y; -- check for a range of values
           SELECT c1 FROM t1 WHERE c1 IS NULL; -- check for NULL values
           SELECT c1 FROM t1 where c1 = 'x' AND c2 < 3; -- filter with multiple conditions
           SELECT c1 FROM t1 WHERE c1='x' OR c3='y'; -- filter with multiple conditions
   
           -- can group related operators with parentheses
           SELECT c1 FROM t1 WHERE (c1='x' OR c1='y') AND c2 = 3;
   
           SELECT c1 FROM t1 WHERE c1 IN ('v1', 'v2', 'v3'); -- can be any one of the values
           SELECT c1 FROM t1 where NOT c2='x';` -- [negate](negate) a condition, same as using `<>`


    * **Wildcards**
        - only for text fields, must use ``LIKE`` for wildcards
        - wildcard searches take longer than other search types
        - do not use wildcards unless really necessary
        - ``%``: any number of characters
        - ``_``: one character, not supported by DB2
        - ``[]``: specify a set of characters, not supported by all DBMSs

        .. code-block:: sql

           SELECT c1 FROM t1 WHERE c1 LIKE '%og'; -- e.g. 'aog' or 'aaaaog'
           SELECT c1 FROM t1 WHERE c1 LIKE '%og%'; -- e.g. 'aaaaogaaaa'
           SELECT c1 FROM t1 WHERE c1 LIKE '__og'; -- e.g. 'aaog'
           SELECT c1 FROM t1 WHERE c1 LIKE '[AB]%'; -- e.g. 'Acd', 'Bcd'
           SELECT c1 FROM t1 WHERE c1 LIKE '[^AB]%'; -- any that does not begin with 'A' or 'B'

    * **Calculated Fields**
        - created within ``SELECT`` statement
        - it is optimal to convert and reformat data on database server than on the client
        - Concatenate: ``+`` or ``||`` based on DBMS
        - remove extra padded spaces: ``TRIM()``, ``RTRIM()``, ``LTRIM()``
        - ``AS``: give alternate name for a field or value, useful with calculated fields,
          cannot be accessed from a separate query
        - can also apply mathematical operations ``+, -, *, /``

        .. code-block:: sql

           SELECT c1 || '(' || c2 || ')' FROM t1;
   
           -- MySQL or MariaDB
           SELECT Concat(c1, '(', c2, ')') FROM t1;
   
           -- use alias
           SELECT c1 || '(' || c2 || ')' AS alias1 FROM t1;
   
           SELECT c1, c2, c1*c2 AS alias1 FROM T1;


    * **Show Structure**
        - ``DESCRIBE t1;``: show structure of the table
    * **Data Grouping**
        - ``GROUP BY``: divide data into logical groups and perform aggregate calculations,
          column cannot be aggregate function or aliases
        - ``HAVING``: filter groups instead of rows, allow all ``WHERE`` options

        .. code-block:: sql

           SELECT c1, COUNT(*) FROM t1 GROUP BY c1;



DCL
---
    * ``GRANT``
    * ``REVOKE``

TCL
---
    * ``COMMIT``
    * ``ROLLBACK``
    * ``SAVEPOINT``

Constraints
-----------
    * ``UNIQUE``
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

Functions
---------
    * special block of code used when querying data
    * not portable and most functions are DBMS specific
    * function names are not case sensitive
    * **Text**
        - ``SUBSTRING()``, ``SUBSTR()``
        - ``UPPER()``, ``LOWER()``
        - ``LEFT()``: characters from left
        - ``RIGHT()``
        - ``LENGTH()``, ``DATALENGTH()``, ``LEN()``
        - ``LTRIM()``, ``RTRIM()``
        - ``SOUNDEX()``: alphanumeric pattern of phonetic representation of text, e.g. searching
          names similar to the queried name
    * **Date & Time**
        - ``CURRENT_DATE``, ``CURDATE()``, ``SYSDATE``, ``GETDATE()``, ``DATE()``
        - ``DATEPART()``, ``STRFTIME()``, ``DATE_PART()``, ``EXTRACT(year FROM)``,
          ``BETWEEN to_date() AND to_date()``, ``YEAR()``
    * **Numeric**
        - ``ABS()``, ``COS()``, ``EXP()``, ``PI()``, ``SIN()``, ``SQRT()``, ``TAN()``
    * **Data Type Conversion**
        - ``CAST()``: DB2, PostgreSQL, SQL Server
        - ``CONVERT()``: MariaDB, MySQL, SQLite
    * **Aggregate**
        - operate on a set of rows and return single value
        - mostly consistent in SQL implementations
        - ``AVG()``: ignore rows with ``NULL``
        - ``COUNT()``: use * to include rows with ``NULL``
        - ``MAX()``: with text data, return the sorted last row by the given column
        - ``MIN()``, ``SUM()``
        - never use ``DISTINCT`` with ``COUNT(*)``

Nested Queries
--------------
    * using results from one query in another
    * will execute the innermost query and use that result to execute outer query
    * use fully qualified column name, ``table.column``, when working with multiple tables
    * use constraints if necessary, as result from one query could be incompatible with other,
      especially when using equal
    * break the queries over multiple lines and indent properly for readability

    .. code-block:: sql

       SELECT t1.c1
       FROM t1
       WHERE t1.c2 IN (SELECT t2.c1
                       FROM t2
                       WHERE t2.c1 = x);



Joins
-----
    * join separate tables on specific common column
    * must specify all tables to be included, and use fully qualified column names
    * resource intensive as joins are processed at runtime
    * most DBMSs have restricted the maximum number of tables per join
    * **Cross Join**
        - Cartesian Product returned without join condition
        - number of rows in first table multiplied by the number of rows in the second table
        - can get incorrect data

        .. code-block:: sql

           SELECT t1.c1, t1.c2, t2.c2 FROM t1, t2;


    * **Inner Join**
        - only common rows will be returned
        - will return duplicates of columns

        .. code-block:: sql

           SELECT t1.c1, t2.c2 FROM t1 INNER JOIN t2 ON t1.c1 = t2.c1;
   
           -- multiple tables
           SELECT c1, c2
           FROM t1
           INNER JOIN t2
               ON t1.c3 = t2.c3
           INNER JOIN t3
               ON t2.c4 = t3.c4
           WHERE t3.c5 = 'x';


    * **Equi Join**
        - just INNER or OUTER JOIN with using only ``=`` as join condition

        .. code-block:: sql

           -- ANSI SQL style
           SELECT t1.c1, t2.c2 FROM t1 JOIN t2 ON t1.c1 = t2.c1;
   
           -- older SQL-89 style, considered deprecated in most databases
           SELECT t1.c1, t2.c2 FROM t1, t2 WHERE t1.c1 = t2.c1;


    * **Self Join**
        - to get data from the same table
        - should use aliases to refer to the same table multiple times

        .. code-block:: sql

   
           SELECT t1.c1, t1.c2, t1.c3
           FROM Table1 AS t1
           INNER JOIN Table2 AS t2
           ON t1.c1 = t2.c1
           WHERE t2.c3 = 'x';


    * **Natural Join**
        - selecting columns that are unique
        - only return one column each from the duplicates

        .. code-block:: sql

           SELECT * FROM t1 NATURAL JOIN t2;


    * **Outer Join**
        - Left: also retrieve non-associated rows from left table
        - Right: also retrieve non-associated rows from right table
        - Full: also retrieve non-associated rows from both tables, same as left join and
          right join combined
        - has various form of syntax based on SQL implementations
        - cannot do in MySQL

        .. code-block:: sql

           -- also retrieve non-associated rows from t1
           SELECT t1.c1, t2.c1 FROM t1 LEFT OUTER JOIN t2 ON t1.c3 = t2.c3;
   
           -- also retrieve non-associated rows from t2
           SELECT t1.c1, t2.c1 FROM t1 RIGHT OUTER JOIN t2 ON t1.c3 = t2.c3;
   
           -- also retrieve non-associated rows from both t1 and t2
           SELECT t1.c1, t2.c1 FROM t1 FULL OUTER JOIN t2 ON t1.c3 = t2.c3;



Union
-----
    * combine columns from ``SELECT`` statements
    * ``SELECT c1 FROM t1 UNION SELECT c2 FROM t2;`` result c1 and c2 in single column
    * number of columns must be same to union, e.g cannot be ``c1,c2 FROM t1 UNION c2 FROM t2``
    * columns must be of same or compatible data type to union, different data types will be
      converted if possible
    * ``SELECT c1 as new_name FROM t1 UNION SELECT c2 FROM t2;`` result column will be ``new_name``

Triggers
--------
    * block of SQL that will be performed when certain action is operated
    * e.g add a row to t2 when certain data is deleted on t1
    * delete trigger: ``DROP TRIGGER trig1;``
    * **Create Trigger**

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


`back to top <#sql>`_

ER Diagrams
===========

* `Entity`_, `Attributes`_, `Relationship`_, `ER to Schema`_
* Entity Relationship Diagrams, helps to convert storage/business requirements into database
  schema
* uses shapes and arrows or graphs to define relationship


Entity
------
    * object to be modeled and stored information about
    * use rectangle shape
    * can define more than one entity in a diagram
    * **Weak Entity**
        - cannot be uniquely identified only by its attributes
        - usually depends on other entities
        - use double-layered rectangle shape
        - always need to have total participation in the relationship

Attributes
----------
    * specific information about an entity
    * use oval shape
    * **Primary Key**
        - uniquely define an entry in the database table
        - use oval shape, text is underlined
    * **Composite Attribute**
        - can be broken into sub-attributes
        - e.g name can be separated into first_name and last_name
    * **Multi-Valued Attribute**
        - can have more than one value
        - use double-layered oval shape
        - e.g a person can have more than one phone numbers
    * **Derived Attribute**
        - can be derived from other attributes
        - use dotted-line oval shape
        - e.g a student's attribute ``passed`` can be derived from attribute ``test_score``

Relationship
------------
    * connect multiple entities in a single diagram
    * use diamond shape
    * connect with single line for partial participation and double line for total
    * **Relationship Attribute**
        - a relationship can have attributes
        - attributes are not stored on entity but on the relationship
    * **Relationship Cardinality**
        - maximum number of times an instance in one entity can relate to instances of another
          entity
        - e.g 1:1, 1:N, N:M
    * **Identifying Relationship**
        - use double-layered diamond shape
        - serves to uniquely identify the weak entity

ER to Schema
------------
    * binary: two entity participating
    * **Create**
        - create relation/table for each regular entity with all simple attributes
        - create relation/table for each weak entity with all simple attributes
        - primary key should be the key of weak entity plus the primary key of its owner
    * **Map 1:1**
        - add primary key of one entity as foreign key in the one that has total participation
        - if both are partial or total, use most convenient approach
    * **Map 1:N**
        - add ``1`` side primary key as foreign key on the ``N`` side
    * **Map M:N**
        - create new relation/table, with primary key as the combination of both entities'
          primary keys, and include any relationship attributes

`back to top <#sql>`_

References & External Resources
===============================

* Forta, B. (2020). Sams Teach Yourself SQL in 10 Minutes, Fifth Edition. Hoboken, NJ: Sams.

`back to top <#sql>`_
