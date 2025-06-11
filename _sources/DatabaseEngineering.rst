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

Isolation
---------
    * Dirty Read: reading data that other transaction has changed but not committed yet
    * Non-repeatable Read: reading data twice and getting different values, where transaction
      has committed
    * Phantom Read: two identical queries getting different results, can always happen with
      WHERE
    * Lost Update: written data lost as it gets updated by other transaction, thus getting a
      different read after write
    * **Isolation Levels**
        - Read Uncommitted: no isolation, any change from the outside is visible to the
          transaction whether committed or not, fast but will get dirty reads
        - Read Committed: each query in a transaction only sees committed changes by others,
          default level for most databases
        - Repeatable Read: row being read by a query remain unchanged while the transaction is
          running
        - Snapshot: each query in a transaction only sees changes committed up to the start of
          the transaction, guaranteed to eliminate read phenomena
        - Serialisable: transactions are run one after another, slow and choosing which one
          goes first is complicated, and guaranteed to eliminate read phenomena
    * each DBMS implements isolation levels differently
    * Pessimistic: using row level locks, table locks, and page locks to avoid lost updates,
      will have pending transactions
    * Optimistic: no locks, tracking if things change and fail the transaction if so, no
      pending transactions

Durability
----------

`back to top <#database-engineering>`_
