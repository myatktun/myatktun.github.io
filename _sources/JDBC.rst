====
JDBC
====

1. `What is JDBC?`_
2. `Drivers`_
3. `Connecting to DB`_
4. `Statements`_
5. `Result Sets`_
6. `Data Types`_
7. `Data Transactions`_
8. `Exceptions`_
9. `Batch Processing`_
10. `Escape Syntax`_
11. `Streaming Data`_

`back to top <#jdbc>`_

What is JDBC?
=============

* `Components`_
* Java Database Connectivity, a standard Java API to access any tabular data
* library includes APIs to connect a database, create and execute SQL or MySQL statements and
queries and view and modify records
* any Java executables can use JDBC driver to access database
* provides same capabilities as ODBC, and thus allowing database-independent code
* latest version is 4.0, with ``java.sql`` and ``javax.sql`` as primary packages
    * has auto database driver loading
    * improve exception handling, better BLOB/CLOB functionality
    * better connection and statement interface
    * national character set support
    * SQL ROWID acess, SQL 2003 XML data type support
* support both two-tier and three-tier processing models
* JDBC API: provide application-to-JDBC Manager connection, use driver manager and
database-specific drivers for transparent connection to heterogeneous databases
* JDBC Driver API: provide JDBC Manager-to-Driver connection, ensure correct driver is used and
support multiple concurrent drivers connected to multiple heterogeneous dbs

Components
----------
    * **DriverManager**
        - a class that manages a list of db drivers
        - match connection requests from application with proper db driver using communication
        subprotocol
        - the first driver that recognizes certain subprotocol will be used for db connection
    * **Driver**
        - an interface that handles communications with db server
        - DriverManager objects will be used to manage Driver objects
    * **Connection**
        - an interface with all methods for contacting a db
        - all communication with db will be through Connection object only
    * **Statement**
        - an interface to submit SQL statements
    * **ResultSet**
        - object that holds data retrieved
        - acts as an iterator to move through data
    * **SQLException**
        - a class that handles any errors

`back to top <#jdbc>`_

Drivers
=======

* ``java.sql`` package in JDK contains classes with behaviors defined
* third-party drivers implement the interface in their database driver
* driver implementations vary depending on operating systems and hardware platforms

JDBC-ODBC Bridge Driver
-----------------------
    * JDBC bridge is used to access ODBC drivers installed on machines
    * require to configure DSN to represent target database
    * only recommneded for experimental use

JDBC-Native API
---------------
    * JDBC API calls are converted to native API calls unique to database
    * vendor-specific driver must be installed
    * speed increase as it eliminates ODBC's overhead

JDBC-Net pure Java
------------------
    * three-tier approach, JDBC->middleware->db
    * JDBC client use standard network sockets
    * middleware server translate socket information into required format and forward it to the
    db
    * flexible driver as a single driver can provide access to multiple databases

Pure Java
---------
    * pure Java-based driver communicate directly with the database through socket connection
    * highest performance driver, usually provided by the vendors
    * extremely flexible and can be downloaded dynamically

`back to top <#jdbc>`_

Connecting to DB
================

* import required packages, register the driver, create URL to database and create connection
object

.. code-block:: java

   import java.sql.*; // standard JDBC package
   
   public class MyJDBC {
       static final String DB_URL = "jdbc:mysql://HOST:PORT/DBNAME";
       static final String USER = "user";
       static final String PASS = "pass";
       static final String QUERY = "SELECT name FROM Table1";
   
       public static void main(String[] args) {
           Connection conn = null;
           Statement stmt = null;
   
           try {
               conn = DriverManager.getConnection(DB_URL, USER, PASS);
               stmt = conn.createStatement();
               ResultSet rs = stmt.executeQuery(QUERY);
   
               while (rs.next()) {
                   System.out.print("Name: " + rs.getInt("name"));
               }
   
           } catch (SQLException e) {
               e.printStackTrace();
           } finally {
               try {
                   if (stmt != null) {
                       stmt.close();
                   }
   
                   if (conn != null) {
                       conn.close();
                   }
               } catch (SQLException e) {
                   e.printStackTrace();
               }
           }
   
       }
   }


* JDBC 4.0 provides feature to auto discover the driver
* but if necessary, there are different ways to register a driver

    .. code-block:: java

       try {
           // configurable and portable driver registration
           Class.forName("oracle.jdbc.driver.OracleDriver");
       } catch (ClassNotFoundException e) {
           e.printStackTrace();
       }
   
       // OR
   
       try {
           // can work around noncompliant JVMs
           Class.forName("oracle.jdbc.driver.OracleDriver").newInstance();
       } catch (ClassNotFoundException e) {
           e.printStackTrace();
       } catch (IllegalAccessException e) {
           e.printStackTrace();
       } catch (InstantiationException e) {
           e.printStackTrace();
       }
   
       // OR
   
       try {
           // if using non-JDK compliant JVM
           Driver myDriver = new oracle.jdbc.driver.OracleDriver();
           DriverManager.registerDriver(myDriver);
       } catch (ClassNotFoundException e) {
           e.printStackTrace();
       }


* can use Properties object to pass username and password

    .. code-block:: java

       import java.util.*;
   
       static final String DB_URL = "jdbc:mysql://HOST:PORT/DBNAME";
       Properties info = new Properties();
       info.put("user", "user");
       info.put("password", "pass");
   
       Connection conn = DriverManager.getConnection(DB_URL, info);


`back to top <#jdbc>`_

Statements
==========

* `Statement`_, `PreparedStatement`_, `CallableStatement`_
* to send PL/SQL commands and receive data from the db
* all parameters in JDBC are represented by '?' symbol, aka parameter marker
* values must be supplied for every parameter before executing SQL statement
* parameter markers start from position 1

Statement
---------
    * interface for general access, when using static SQL statements at runtime
    * does not accept parameters
    * ``execute(String)``: return true if ResultSet object can be retrieved, use to execute SQL
    DDL statements or dynamic SQL
    * ``executeUpdate(String)``: return number of rows affected by the execution, use to execute
    INSERT, UPDATE or DELETE
    * ``executeQuery(String)``: return ResultSet object, use to execute SELECT
    * creating the Connection object first will close the Statement object, but should
    explicitly close the Statement object for proper cleanup

    .. code-block:: java

       // create Statement object
       Statement stmt = null;
   
       try {
           stmt = conn.createStatement();
       } catch (SQLException e) {
       } finally {
           try {
               if (stmt != null) {
                   stmt.close();
               }
           } catch (SQLException e) {
           }
       }



PreparedStatement
-----------------
    * when SQL statements will be used many times
    * accept input parameters at runtime
    * extend Statement interface
    * parameter values must be bind before executing
    * ``IN``: unknown value parameter when SQL statement is created, bind values with set methods

    .. code-block:: java

       PreparedStatement pstmt = null;
   
       try {
           String QUERY = "UPDATE Table1 SET name=? where id=?";
           pstmt = conn.preparedStatement(SQL);
           // "name" of "id" with "102" will be set to "foo"
           pstmt.setInt(1, "foo");
           pstmt.setInt(2, 102);
       } catch (SQLException e) {
       } finally {
           try {
               if (pstmt != null) {
                   pstmt.close();
               }
           } catch (SQLException e) {
           }
       }



CallableStatement
-----------------
    * to access database stored procedures
    * accept input parameters at runtime
    * ``IN``: unknown value parameter when SQL statement is created, bind values with set methods
    * ``OUT``: parameter value is supplied by SQL statement it returns, retrieve with get methods
    * ``INOUT``: provide both input and output values, bind with set methods and retrieve with
    get methods
    * parameter values must be bind before executing
    * ``registerOutParameter()``: bind JDBC data type to the data type returned by the stored
    procedure, must use with `OUT` and `INOUT` params

    .. code-block:: java

       CallableStatement cstmt = null;
   
       try {
           // "getName" procedure must be created in the database first
           String storedProcedure = "{call getName (?, ?)}";
           cstmt = conn.prepareCall(storedProcedure);
   
           cstmt.setInt(1, 100); // set ID
           cstmt.registerOutParameter(2, java.sql.Types.VARCHAR); // register second OUT param
   
           cstmt.execute();
   
           System.out.print("ID: 100 name is " + cstmt.getString(2));
       } catch (SQLException e) {
       } finally {
           try {
               if (cstmt != null) {
                   cstmt.close();
               }
           } catch (SQLException e) {
           }
       }


`back to top <#jdbc>`_

Result Sets
===========

* `Types`_, `Concurrency`_, `Navigate`_, `Get`_, `Update`_
* in ``java.sql.ResultSet``, returned data from a query
* ResultSet object has cursor pointing to the current row in the result set
* result set refers to the row an column data contained in a ResultSet object
* connection methods to create statements with desired ResultSet
    * ``createStatement(int RSType, int RSConcurrency)``
    * ``preparedStatement(String SQL, int RSType, int RSConcurrency)``
    * ``prepareCall(String SQL, int RSType, int RSConcurrency)``

Types
-----
    * ``ResultSet.TYPE_FORWARD_ONLY``: cursor can move only forward, default if ResultSet type is
    not specified
    * ``ResultSet.TYPE_SCROLL_INSENSITIVE``: cursor can scroll forward and backward, changes made
    by others to the db after the result set was created do not affect it
    * ``ResultSet.TYPE_SCROLL_SENSITIVE``: cursor can scroll forward and backward, changes made
    by others to the db after the result set was created affect the it

Concurrency
-----------
    * ``ResultSet.CONCUR_READ_ONLY``: create read-only result set, default one
    * ``ResultSet.CONCUR_UPDATABLE``: create updatable result set

Navigate
--------
    * to move the cursor around
    * all methods throw ``SQLException``
    * ``beforeFirst()``: move cursor just before the first row
    * ``afterLast()``: move cursor just after the last row
    * ``first()``: move cursor to the first row, return true if valid
    * ``last()``: move cursor to the last row, return true if valid
    * ``absolute(int)``: move cursor to specified row, return true if valid
    * ``relative(int)``: move cursor forward or backward specific rows, from where it is
    currently pointing, return true on valid
    * ``previous()``: move cursor to previous row, return true if valid
    * ``next()``: move cursor to next row, return true if valid
    * ``getRow()``: return current row number
    * ``moveToInsertRow()``: move cursor to special row that can be used to insert new row,
    current cursor location is saved
    * ``moveToCurrentRow()``: move cursor back to current row if it is at the insert row

Get
---
    * to view data in the columns of current row pointed by the cursor
    * get methods for possible data types are available, and each get method accepts column
    name or column index
    * column index starts at 1
    * e.g use ``getInt(String)`` or ``getInt(int)`` to view column that contains an int
    * there are methods for eight primitive data types, common types, such as String, and SQL
    data types, such as `java.sql.Date`, `java.sql.Time`, `java.sql.TimeStamp`, `java.sql.Clob`
    or `java.sql.Blob`

Update
------
    * to update data in the columns of current row, and in the underlying db as well
    * tables should have Primary Key set properly for it to be updatable
    * update methods for possible data types are available, and each update method accepts
    column name or column index
    * e.g use ``updateString(String, String)`` or ``updateString(int, String)`` to update column
    that contains String
    * there are methods for eight primitive data types, common types, such as String, Object,
    URL, and SQL data types
    * updating only changes the columns of current row in the ResultSet object, not the
    underlying db
    * additional methods must be invoked to update the changes in the database
    * ``updateRow()``: update current row as well as the row in the database
    * ``deleteRow()``: delete current row from the db
    * ``refreshRow()``: refresh data in the result set to reflect changes in the db
    * ``cancelRowUpdates()``: cancel updates made on current row
    * ``insertRow()``: insert row into the db, can only be invoked if the cursor is pointing to
    the insert row

.. code-block:: java

   Statement stmt = conn.createStatement(ResultSet.TYPE_SCROLL_INSENSITIVE,
           ResultSet.CONCUR_READ_ONLY);
   ResultSet rs = stmt.executeQuery(QUERY);
   rs.last(); // move cursor to the last row
   System.out.println("Name: " + rs.getString("name")); // view data in the last row
   
   rs.first(); // move cursor to the first row
   System.out.println("Name: " + rs.getString("name")); // view data in the first row
   
   rs.next(); // move cursor to the next row
   System.out.println("Name: " + rs.getString("name")); // view data
   
   // change statement to be updatable
   stmt = conn.createStatement(ResultSet.TYPE_SCROLL_INSENSITIVE,
           ResultSet.CONCUR_UPDATABLE);
   rs = stmt.executeQuery(QUERY);
   
   // making sure to start with the first row
   rs.beforeFirst();
   while (rs.next()) {
       int newID = rs.getInt("id") + 2;
       rs.updateDouble("id", newID);
       rs.updateRow();
   }
   
   // to insert new row
   rs.moveToInsertRow();
   rs.updateString("name", "foo");
   rs.updateInt("age", 99);
   rs.insertRow(); // insert the new row into db


`back to top <#jdbc>`_

Data Types
==========

* Java data type is converted to appropriate JDBC type before being sent to the database
* default mappings, such as Java int to SQL INTEGER, are used for consistency between drivers
* e.g VARCHAR -> java.lang.String, BIT -> boolean, INTEGER -> int, BIGINT -> long
* BINARY -> byte[], TIMESTAMP -> java.sql.Timestamp, BLOB -> java.sql.Blob
* JDBC 3.0 has support for BLOB, CLOB, ARRAY and REF data types
* set and update methods, such as ``setString()`` and ``updateBLOB()``, convert Java types to JDBC
data types
* ``setObject()`` and ``updateObject()`` methods map Java type to JDBC data type
* ResultSet object also provides set and get methods for each data type
* more information about data types from [Oracle](https://docs.oracle.com/cd/E19830-01/819-4721/beajw/index.html) and [Tutorials Point](https://www.tutorialspoint.com/jdbc/jdbc-data-types.htm)

.. code-block:: java

   java.util.Date javaDate = new java.util.Date();
   long javaTime = javaDate.getTime();
   System.out.println("Java Date: " + javaDate.toString());
   
   // java.sql.Date maps to SQL DATE type
   java.sql.Date sqlDate = new java.sql.Date(javaTime);
   System.out.println("SQL Date: " + sqlDate.toString());
   
   // java.sql.Time maps to SQL TIME type
   java.sql.Time sqlTime = new java.sql.Time(javaTime);
   System.out.println("SQL Time: " + sqlTime.toString());
   
   // java.sql.Timestamp maps to SQL TIMESTAMP type
   java.sql.Timestamp sqlTimestamp = new java.sql.Timestamp(javaTime);
   System.out.println("SQL Timestamp: " + sqlTimestamp.toString());



NULL values
-----------
    * SQL NULL values must be handled differently, as they are not same as Java's
    * avoid using get methods that return primitive data types
    * use wrapper classes for primitive data types
    * use ResultSet object's ``wasNull()`` to test if the value returned by get methods should be
    set to null or appropriate value

    .. code-block:: java

       Statement stmt = conn.createStatement();
       ResultSet rs = stmt.executeQuery(SQL);
   
       int id = rs.getInt(1);
       if (rs.wasNull()) {
           id = 0;
       }


`back to top <#jdbc>`_

Data Transactions
=================

* `Savepoint`_
* every SQL statement is auto committed to the database on completion by default
* transactions can be managed to increase performance, maintain integrity and use distributed
transactions
* in a group of SQL statements, if any statement fails, the whole transaction fails
* use ``setAutoCommit(false)`` on Connection object to enable manual transaction
* ``commit()`` to commit changes and ``rollback()`` to roll back updates made to the database

.. code-block:: java

   try {
       conn.setAutoCommit(false);
       stmt.executeUpdate(SQL);
       conn.commit(); // commit changes if no error
   } catch (SQLException e) {
       conn.rollback(); // roll back on error
   }



Savepoint
---------
    * JDBC 3.0 support Savepoint interface for additional transaction control
    * can rollback to undo all changes or only the ones made after the savepoint
    * ``setSavepoint(String)``: create new savepoint, return Savepoint object
    * ``releaseSavepoint(Savepoint)``: delete given Savepoint
    * ``rollback(String savePointName)``: roll back to given savepoint

    .. code-block:: java

       try {
           Savepoint s1 = conn.setSavepoint("Savepoint1");
           stmt.executeUpdate(SQL);
           conn.commit(); // commit changes if no error
       } catch (SQLException e) {
           conn.rollback(s1); // roll back to "Savepoint1" on error
       }


`back to top <#jdbc>`_

Exceptions
==========

* most in ``java.sql.SQLException``, which can occur in driver and database
* ``getErrorCode()``: get error number
* ``getMessage()``: get JDBC driver error message, can be a number or message for database error
* ``getSQLState()``: get XOPEN SQLstate string, no useful info for JDBC driver error, five-digit
XOPEN SQLstate code for db error, can also return null
* ``getNextException()``: get next Exception object in the exception chain
* ``printStackTrace()``, ``printStackTrace(PrintStream)``, ``printStackTrace(PrintWriter)``: print
current exception or throwable and backtrace to std error or given print stream or writer

`back to top <#jdbc>`_

Batch Processing
================

* group related SQL statements and submit as one call to the database
* can reduce amount of communication and improve performance
* JDBC drivers are not required to support batch processing
* ``DatabaseMetaData.supportsBatchUpdates()``: return true if the database support the feature
* ``addBatch()``: add individual statements to the batch
* ``executeBatch()``: execute all grouped statements
* ``clearBatch()``: remove all statements added with ``addBatch()``, cannot select which to remove

.. code-block:: java

   Statement stmt = conn.createStatement();
   conn.setAutoCommit(false);
   
   stmt.addBatch(SQL1);
   stmt.addBatch(SQL2);
   
   int[] count = stmt.executeBatch(); // to hold returned values
   conn.commit(); // apply changes


`back to top <#jdbc>`_

Escape Syntax
=============

* allow to use database specific features unavailable when using only standard JDBC methods
* general SQL escape syntax format: ``{keyword 'parameters'}``
* date: ``{d 'yyyy-mm-dd'}``
* time: ``{t 'hh:mm:ss'}``
* timestamp: ``{ts 'yyyy-mm-dd hh:mm:ss'}``
* escape: ``{escape 'ESCAPE_WORD'}``
* scalar functions: ``{fn length('foo bar')}``
* calling stored procedure: ``{call procedure1(?)}``, procedure with IN param,
``{? = call procedure1(?)}``, procedure with IN and return OUT param
* outer joins: ``{oj outer-koin}``

.. code-block:: java

   SQL = "INSERT INTO Table1 ({d '2000-1-12'})"; // insert date
   SQL = "INSERT INTO Table1 ({t '18:15:30'})"; // insert time
   SQL = "INSERT INTO Table1 ({ts '2000-1-12 18:15:30'})"; // insert timestamp
   SQL = "SELECT symbol FROM MathSymbols WHERE symbol LIKE '\%' {escape '\'}";
   sql = "SELECT Employees FROM {oj ThisTable RIGHT OUTER JOIN ThatTable on id = '100'}";


`back to top <#jdbc>`_

Streaming Data
==============

* PreparedStatement object can use input and output streams to supply parameter data
* can place files into database columns that hold large values, such as CLOB and BLOB
* ``setAsciiStream()``: to supply large ASCII values
* ``setCharacterStream()``: to supply large UNICODE values
* ``setBinaryStream()``: to supply large Binary values
* each method also require file size as parameter

.. code-block:: java

   PreparedStatement pstmt = null;
   String XML_INSERT_QUERY = "INSERT INTO XML_Data VALUES (?,?)";
   String XML_DATA = "<Employee><id>100</id><name>foo</name></Employee>";
   String XML_QUERY = "SELECT Data FROM XML_Data WHERE id=100";
   
   try {
       stmt = conn.createStatement();
       pstmt = conn.prepareStatement(XML_INSERT_QUERY);
   
       ByteArrayInputStream bis = new ByteArrayInputStream(XML_DATA.getBytes());
       pstmt.setInt(1, 100);
       pstmt.setAsciiStream(2, bis, XML_DATA.getBytes().length);
       pstmt.execute();
   
       bis.close();
   
       ResultSet rs = stmt.executeQuery(XML_QUERY);
   
       if (rs.next()) {
           InputStream is = rs.getAsciiStream(1);
           int c;
           ByteArrayOutputStream bos = new ByteArrayOutputStream();
   
           while ((c = is.read()) != -1)
               bos.write(c);
   
           System.out.println(bos.toString());
       }
   
       rs.close();
   
   } catch (SQLException | IOException e) {
       e.printStackTrace();
   }


`back to top <#jdbc>`_
