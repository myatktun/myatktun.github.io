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

`back to top <#database-engineering>`_
