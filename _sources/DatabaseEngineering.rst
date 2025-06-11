====================
Database Engineering
====================

1. `ACID`_

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
        - enforce referential integrity with foreign keys
        - atomicity also ensures consistency, and isolation level affects data consistency
        - no eventual consistency if data is corrupted
    * **Read**
        - consistent even in a cluster of multiple instances
        - reads will have eventual consistency by getting the correct updated values

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
        - Read Uncommitted: no isolation, any change from the outside is visible to the
          transaction whether committed or not, fast but will get dirty reads
        - Read Committed: each query in a transaction only sees committed changes by others,
          default level for most databases
        - Repeatable Read: row being read by a query remain unchanged while the transaction is
          running, eliminate Phantom Read in Postgres
        - Serializable: transactions are run one after another, slow and choosing which one
          goes first is complicated, and guaranteed to eliminate read phenomena
        - Snapshot: each query in a transaction only sees changes committed up to the start of
          the transaction, guaranteed to eliminate read phenomena
    * each DBMS implements isolation levels differently
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
