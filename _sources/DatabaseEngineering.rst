====================
Database Engineering
====================

1. `ACID`_
2. `Internals`_

`back to top <#database-engineering>`_

ACID
====

* `Transaction`_, `Atomicity`_, `Consistency`_, `Isolation`_, `Durability`_
* four critical properties of relational database systems

Transaction
-----------
    * a collection of SQL queries treated as one unit of work that cannot be split
    * usually used to change and modify data, but can have read-only transaction
    * long transactions are not recommended in general, e.g. 50,000 queries in a transaction
    * user defined transaction, or implicitly defined built-in transaction by the system
    * with ``autocommit=ON``, statements not executed in a transaction will be wrapped in its own
      transaction and committed immediately, thus rollback will not undo
    * **Lifespan**
        - BEGIN: start a new transaction with multiple queries
        - COMMIT: persist the changes to the disk
        - ROLLBACK: undo the changes, optimally in memory
        - unexpected end or crash should cause a ROLLBACK when the database restarts

Atomicity
---------
    * all queries in a transaction must succeed
    * failure of one or more queries, or database crash before commit should cause rollback on
      the previous successful ones
    * lack of atomicity leads to inconsistency

Consistency
-----------
    * **Data**
        - consistent in disk, defined by the user with data model
        - has referential integrity with foreign keys
        - atomicity also ensures consistency, and isolation level affects data consistency
        - no eventual consistency if data is corrupted
    * **Read**
        - consistent even in a cluster of multiple instances
        - reads will have eventual consistency by getting the correct updated values
    * there will be inconsistency when a data is in two places, e.g. using a cache and not
      updating it

Isolation
---------
    * **Read Phenomena**
        - Dirty Read: reading data that other transaction has changed but not committed yet
        - Non-repeatable Read: reading data twice and getting different values, where other
          transaction has committed
        - Phantom Read: two identical queries getting different results, can always happen
          with WHERE
        - Lost Update: written data lost as it gets updated by other transaction, thus getting
          a different read after write
    * **Isolation Levels**
        - the transaction will always see the changes it makes regardless of the level,
          isolation level only applies to other concurrent transactions
        - Read Uncommitted: no isolation, any change from the outside is visible to the
          transaction whether committed or not, fast but will get dirty reads
        - Read Committed: each query in a transaction sees committed changes by others,
          default level for most databases
        - Repeatable Read: row being read by a query remain unchanged while the transaction is
          running, eliminate Phantom Read in Postgres
        - Serializable: transactions are run one after another, slow and choosing which one
          goes first is complicated, and guaranteed to eliminate read phenomena
        - Snapshot: each query in a transaction only sees changes committed up to the start of
          the transaction, guaranteed to eliminate read phenomena
        - each DBMS implements isolation levels differently
        - Repeatable Read isolation level in Postgres and MySQL is implemented as snapshot
          isolation, and phantom reads are not possible
    * Pessimistic: using row level locks, table locks, and page locks to avoid lost updates,
      will have pending transactions
    * Optimistic: no locks, tracking if things change and fail the transaction if so, no
      pending transactions

Durability
----------
    * changes made by committed transactions must be persisted in a durable non-volatile
      storage, even after system restart
    * many databases write changes to memory first, then snapshot and write to disk in the
      background
    * the system is durable only when a commit is successful and written to disk, thus commit
      speeds are critical
    * **WAL**
        - Write-Ahead Logging
        - changes are persisted in WAL first, without really updating the data table yet
        - if there is a crash, the updated state can be rebuilt from WAL entries
    * **Asynchronous Snapshot**
        - changes are kept in memory, and snapshot to the disk asynchronously in the
          background
    * **AOF**
        - Append-Only File, similar to WAL and used by Redis
        - lightweight and fast way of storing data
    * **OS Cache**
        - write request in OS usually goes to the OS cache, then a batch of writes are flushed
          to disk
        - writes in the OS cache will be lost if the OS crashes
        - Fsync OS command forces writes to always go to the disk, but it is expensive and
          slows down commits

`back to top <#database-engineering>`_

Internals
=========

* `Row ID`_, `Page`_, `Heap`_, `Index`_, `Row Store`_, `Column Store`_
* in relational databases, data is stored in the concept of tables with rows and columns

Row ID
------
    * most databases create internal and system maintained row id, instead of using the user
      defined fields
    * also called Tuple ID in Postgres
    * in MySQL, the primary key becomes row id

Page
----
    * a fixed size memory location where rows are stored
    * a page can store multiple rows, with page size being 8KB in Postgres and 16KB in MySQL
    * the database reads a page or more, retrieving multiple rows in a single I/O
    * a database query can be slow or fast depending on how many pages are read, i.e. the
      lesser the I/O operations, the faster the queries

Heap
----
    * data structure that stores the table with all its pages one after another
    * actual data, including everything, is stored in heap
    * traversing the heap is expensive as much data needs to be read to find the target
    * indexes tell what part/pages of the heap needs to be read

Index
-----
    * data structure with pointers to the heap, and is used to quickly search
    * gives exact page to fetch in the heap
    * can index one column or more
    * stored as pages and cost I/O to get the entries of the index
    * the smaller the index, the better it can fit in memory, resulting in faster search
    * mostly b-trees are used to implement index
    * **Clustered Index**
        - heap table organised around a single index
        - also called Index Organised Table
        - primary key is usually a clustered index, unless specified
        - MySQL InnoDB always have a primary key, and other indexes point to the primary key
          value
    * Postgres only have secondary indexes, and all indexes point directly to row id which is
      in the heap

Row Store
---------
    * row-oriented database where tables are stored as rows in disk
    * a single block I/O read to the table fetches multiple rows with all their columns
    * more I/O is required to find a row in a table, but all columns for the row is already
      in memory when it is found
    * when querying for specific column, unnecessary column values are also retrieved on each
      I/O, e.g. `select sum(salary) from emp;` will get every column just to get salary value
    * optimal for read/write and online transactional processing
    * compression and aggregation aren't efficient

Column Store
------------
    * column-oriented database where tables are stored as columns in disk
    * a single block I/O read to the table fetches multiple columns with all matching rows
    * less I/O is required to get more values of a given column, but multiple columns require
      more I/O, e.g. `select *` will cause multiple I/O to get every column values
    * row id is duplicated in every column, and it is used to get other column values
    * queries such as ``select sum(salary) from emp;`` need less I/O than that in row store,
      since the salary column values are stored in same block
    * writes are slower, but optimal for online analytical processing
    * compression and aggregation are efficient

`back to top <#database-engineering>`_
