## Creating and modifying a database {#creating-and-modifying-a-database}

Adobe AIR 1.0 and later

Before your application can add or retrieve data, there must be a database with tables defined in it that your application can access. Described here are the tasks of creating a database and creating the data structure within a database. While these tasks are less frequently used than data insertion and retrieval, they are necessary for most applications.

**More Help topics**

[Mind the Flex: Updating an existing AIR database](http://www.mindtheflex.com/?p=83)

**Creating a database**

Adobe AIR 1.0 and later

To create a database file, you first create a SQLConnection instance. You call its open() method to open it in synchronous execution mode, or its openAsync() method to open it in asynchronous execution mode. The open() and openAsync() methods are used to open a connection to a database. If you pass a File instance that refers to a non- existent file location for the reference parameter (the first parameter), the open() or openAsync() method creates a database file at that file location and open a connection to the newly created database.

Whether you call the open() method or the openAsync() method to create a database, the database file’s name can be any valid filename, with any filename extension. If you call the open() or openAsync() method with null for the reference parameter, a new in-memory database is created rather than a database file on disk.

The following code listing shows the process of creating a database file (a new database) using asynchronous execution mode. In this case, the database file is saved in the [“Pointing to the application storage directory” on page 672](..\chapter_38_working_with_the_file_system\using_the_air_file_system_api.md#696154181379824-_bookmark408), with the filename “DBSample.db”:

import flash.data.SQLConnection; import flash.events.SQLErrorEvent; import flash.events.SQLEvent; import flash.filesystem.File;

var conn:SQLConnection = new SQLConnection();

conn.addEventListener(SQLEvent.OPEN, openHandler); conn.addEventListener(SQLErrorEvent.ERROR, errorHandler);

// The database file is in the application storage directory var folder:File = File.applicationStorageDirectory;

var dbFile:File = folder.resolvePath(&quot;DBSample.db&quot;); conn.openAsync(dbFile);

function openHandler(event:SQLEvent):void

{

trace(&quot;the database was created successfully&quot;);

}

function errorHandler(event:SQLErrorEvent):void

{

trace(&quot;Error message:&quot;, event.error.message); trace(&quot;Details:&quot;, event.error.details);

}

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) creationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.data.SQLConnection; import flash.events.SQLErrorEvent; import flash.events.SQLEvent; import flash.filesystem.File;

private function init():void

{

var conn:SQLConnection = new SQLConnection();

conn.addEventListener(SQLEvent.OPEN, openHandler); conn.addEventListener(SQLErrorEvent.ERROR, errorHandler);

// The database file is in the application storage directory var folder:File = File.applicationStorageDirectory;

var dbFile:File = folder.resolvePath(&quot;DBSample.db&quot;);

conn.openAsync(dbFile);

}

private function openHandler(event:SQLEvent):void

{

trace(&quot;the database was created successfully&quot;);

}

private function errorHandler(event:SQLErrorEvent):void

{

}

]]&gt;

trace(&quot;Error message:&quot;, event.error.message); trace(&quot;Details:&quot;, event.error.details);

&lt;/mx:Script&gt;

&lt;/mx:WindowedApplication&gt;

**_Note:_ **_Although the File class lets you point to a specific native file path, doing so can lead to applications that will not work across platforms. For example, the path C:\Documents and Settings\joe\test.db only works on Windows. For these reasons, it is best to use the static properties of the File class such as File.applicationStorageDirectory, as well as the resolvePath() method (as shown in the previous example). For more information, see_

_“Paths of File objects” on_

_page 668_

_._

To execute operations synchronously, when you open a database connection with the SQLConnection instance, call the open() method. The following example shows how to create and open a SQLConnection instance that executes its operations synchronously:

import flash.data.SQLConnection; import flash.errors.SQLError; import flash.filesystem.File;

var conn:SQLConnection = new SQLConnection();

// The database file is in the application storage directory var folder:File = File.applicationStorageDirectory;

var dbFile:File = folder.resolvePath(&quot;DBSample.db&quot;);

try

{

}

conn.open(dbFile);

trace(&quot;the database was created successfully&quot;);

catch (error:SQLError)

{

trace(&quot;Error message:&quot;, error.message); trace(&quot;Details:&quot;, error.details);

}

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) creationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.data.SQLConnection; import flash.errors.SQLError; import flash.filesystem.File;

private function init():void

{

var conn:SQLConnection = new SQLConnection();

// The database file is in the application storage directory var folder:File = File.applicationStorageDirectory;

var dbFile:File = folder.resolvePath(&quot;DBSample.db&quot;);

try

{

}

conn.open(dbFile);

trace(&quot;the database was created successfully&quot;);

catch (error:SQLError)

{

}

]]&gt;

trace(&quot;Error message:&quot;, error.message); trace(&quot;Details:&quot;, error.details);

}

&lt;/mx:Script&gt;

&lt;/mx:WindowedApplication&gt;

### Creating database tables {#creating-database-tables}

Adobe AIR 1.0 and later

Creating a table in a database involves executing a SQL statement on that database, using the same process that you use to execute a SQL statement such as SELECT, INSERT, and so forth. To create a table, you use a CREATE TABLE statement, which includes definitions of columns and constraints for the new table. For more information about executing SQL statements, see

“Working with SQL statements” on page 729

.

The following example demonstrates creating a table named “employees” in an existing database file, using asynchronous execution mode. Note that this code assumes there is a SQLConnection instance named conn that is already instantiated and is already connected to a database.

import flash.data.SQLConnection; import flash.data.SQLStatement; import flash.events.SQLErrorEvent; import flash.events.SQLEvent;

// ... create and open the SQLConnection instance named conn ...

var createStmt:SQLStatement = new SQLStatement(); createStmt.sqlConnection = conn;

var sql:String =

&quot;CREATE TABLE IF NOT EXISTS employees (&quot; +

*   empId INTEGER PRIMARY KEY AUTOINCREMENT, &quot; +
*   firstName TEXT, &quot; + &quot; lastName TEXT, &quot; +
*   salary NUMERIC CHECK (salary &gt; 0)&quot; + &quot;)&quot;;

createStmt.text = sql;

createStmt.addEventListener(SQLEvent.RESULT, createResult); createStmt.addEventListener(SQLErrorEvent.ERROR, createError);

createStmt.execute();

function createResult(event:SQLEvent):void

{

trace(&quot;Table created&quot;);

}

function createError(event:SQLErrorEvent):void

{

trace(&quot;Error message:&quot;, event.error.message); trace(&quot;Details:&quot;, event.error.details);

}

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) creationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.data.SQLConnection; import flash.data.SQLStatement; import flash.events.SQLErrorEvent; import flash.events.SQLEvent;

private function init():void

{

// ... create and open the SQLConnection instance named conn ...

var createStmt:SQLStatement = new SQLStatement(); createStmt.sqlConnection = conn;

var sql:String =

&quot;CREATE TABLE IF NOT EXISTS employees (&quot; +

*   *   empId INTEGER PRIMARY KEY AUTOINCREMENT, &quot; +
    *   firstName TEXT, &quot; + &quot; lastName TEXT, &quot; +
    *   salary NUMERIC CHECK (salary &gt; 0)&quot; + &quot;)&quot;;

createStmt.text = sql;

createStmt.addEventListener(SQLEvent.RESULT, createResult); createStmt.addEventListener(SQLErrorEvent.ERROR, createError);

createStmt.execute();

}

private function createResult(event:SQLEvent):void

{

trace(&quot;Table created&quot;);

}

private function createError(event:SQLErrorEvent):void

{

}

]]&gt;

trace(&quot;Error message:&quot;, event.error.message); trace(&quot;Details:&quot;, event.error.details);

&lt;/mx:Script&gt;

&lt;/mx:WindowedApplication&gt;

The following example demonstrates how to create a table named “employees” in an existing database file, using synchronous execution mode. Note that this code assumes there is a SQLConnection instance named conn that is already instantiated and is already connected to a database.

import flash.data.SQLConnection; import flash.data.SQLStatement; import flash.errors.SQLError;

// ... create and open the SQLConnection instance named conn ...

var createStmt:SQLStatement = new SQLStatement(); createStmt.sqlConnection = conn;

var sql:String =

&quot;CREATE TABLE IF NOT EXISTS employees (&quot; +

*   empId INTEGER PRIMARY KEY AUTOINCREMENT, &quot; +
*   firstName TEXT, &quot; + &quot; lastName TEXT, &quot; +
*   salary NUMERIC CHECK (salary &gt; 0)&quot; + &quot;)&quot;;

createStmt.text = sql;

try

{

}

createStmt.execute(); trace(&quot;Table created&quot;);

catch (error:SQLError)

{

trace(&quot;Error message:&quot;, error.message); trace(&quot;Details:&quot;, error.details);

}

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) creationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.data.SQLConnection; import flash.data.SQLStatement; import flash.errors.SQLError;

private function init():void

{

// ... create and open the SQLConnection instance named conn ...

var createStmt:SQLStatement = new SQLStatement(); createStmt.sqlConnection = conn;

var sql:String =

&quot;CREATE TABLE IF NOT EXISTS employees (&quot; +

*   *   empId INTEGER PRIMARY KEY AUTOINCREMENT, &quot; +
    *   firstName TEXT, &quot; + &quot; lastName TEXT, &quot; +
    *   salary NUMERIC CHECK (salary &gt; 0)&quot; + &quot;)&quot;;

createStmt.text = sql;

try

{

}

createStmt.execute(); trace(&quot;Table created&quot;);

catch (error:SQLError)

{

}

]]&gt;

trace(&quot;Error message:&quot;, error.message); trace(&quot;Details:&quot;, error.details);

}

&lt;/mx:Script&gt;

&lt;/mx:WindowedApplication&gt;