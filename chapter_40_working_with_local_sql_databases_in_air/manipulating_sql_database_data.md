## Manipulating SQL database data {#manipulating-sql-database-data}

Adobe AIR 1.0 and later

There are some common tasks that you perform when you’re working with local SQL databases. These tasks include connecting to a database, adding data to tables, and retrieving data from tables in a database. There are also several issues you’ll want to keep in mind while performing these tasks, such as working with data types and handling errors.

Note that there are also several database tasks that are things you’ll deal with less frequently, but will often need to do before you can perform these more common tasks. For example, before you can connect to a database and retrieve data from a table, you’ll need to create the database and create the table structure in the database. Those less-frequent initial setup tasks are discussed in

“Creating and modifying a database” on page 718

.

You can choose to perform database operations asynchronously, meaning the database engine runs in the background and notifies you when the operation succeeds or fails by dispatching an event. You can also perform these operations synchronously. In that case the database operations are performed one after another and the entire application (including updates to the screen) waits for the operations to complete before executing other code. For more information on working in asynchronous or synchronous execution mode, see

“Using synchronous and asynchronous

database operations” on page 753

.

**Connecting to a database**

Adobe AIR 1.0 and later

Before you can perform any database operations, first open a connection to the database file. A SQLConnection instance is used to represent a connection to one or more databases. The first database that is connected using a SQLConnection instance is known as the “main” database. This database is connected using the open() method (for synchronous execution mode) or the openAsync() method (for asynchronous execution mode).

If you open a database using the asynchronous openAsync() operation, register for the SQLConnection instance’s open event in order to know when the openAsync() operation completes. Register for the SQLConnection instance’s error event to determine if the operation fails.

The following example shows how to open an existing database file for asynchronous execution. The database file is named “DBSample.db” and is located in the user’s [“Pointing to the application storage directory” on page 672](..\chapter_38_working_with_the_file_system\using_the_air_file_system_api.md#696154181379824-_bookmark408).

import flash.data.SQLConnection; import flash.data.SQLMode;

import flash.events.SQLErrorEvent; import flash.events.SQLEvent; import flash.filesystem.File;

var conn:SQLConnection = new SQLConnection();

conn.addEventListener(SQLEvent.OPEN, openHandler); conn.addEventListener(SQLErrorEvent.ERROR, errorHandler);

// The database file is in the application storage directory var folder:File = File.applicationStorageDirectory;

var dbFile:File = folder.resolvePath(&quot;DBSample.db&quot;); conn.openAsync(dbFile, SQLMode.UPDATE);

function openHandler(event:SQLEvent):void

{

trace(&quot;the database opened successfully&quot;);

}

function errorHandler(event:SQLErrorEvent):void

{

trace(&quot;Error message:&quot;, event.error.message); trace(&quot;Details:&quot;, event.error.details);

}

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) creationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.data.SQLConnection; import flash.data.SQLMode;

import flash.events.SQLErrorEvent; import flash.events.SQLEvent; import flash.filesystem.File;

private function init():void

{

var conn:SQLConnection = new SQLConnection();

conn.addEventListener(SQLEvent.OPEN, openHandler); conn.addEventListener(SQLErrorEvent.ERROR, errorHandler);

// The database file is in the application storage directory var folder:File = File.applicationStorageDirectory;

var dbFile:File = folder.resolvePath(&quot;DBSample.db&quot;);

conn.openAsync(dbFile, SQLMode.UPDATE);

}

private function openHandler(event:SQLEvent):void

{

trace(&quot;the database opened successfully&quot;);

}

private function errorHandler(event:SQLErrorEvent):void

{

}

]]&gt;

trace(&quot;Error message:&quot;, event.error.message); trace(&quot;Details:&quot;, event.error.details);

&lt;/mx:Script&gt;

&lt;/mx:WindowedApplication&gt;

The following example shows how to open an existing database file for synchronous execution. The database file is named “DBSample.db” and is located in the user’s [“Pointing to the application storage directory” on page 672](..\chapter_38_working_with_the_file_system\using_the_air_file_system_api.md#696154181379824-_bookmark408).

import flash.data.SQLConnection; import flash.data.SQLMode; import flash.errors.SQLError; import flash.filesystem.File;

var conn:SQLConnection = new SQLConnection();

// The database file is in the application storage directory var folder:File = File.applicationStorageDirectory;

var dbFile:File = folder.resolvePath(&quot;DBSample.db&quot;);

try

{

}

conn.open(dbFile, SQLMode.UPDATE); trace(&quot;the database opened successfully&quot;);

catch (error:SQLError)

{

trace(&quot;Error message:&quot;, error.message); trace(&quot;Details:&quot;, error.details);

}

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) creationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.data.SQLConnection; import flash.data.SQLMode; import flash.errors.SQLError; import flash.filesystem.File;

private function init():void

{

var conn:SQLConnection = new SQLConnection();

// The database file is in the application storage directory var folder:File = File.applicationStorageDirectory;

var dbFile:File = folder.resolvePath(&quot;DBSample.db&quot;);

try

{

}

conn.open(dbFile, SQLMode.UPDATE); trace(&quot;the database opened successfully&quot;);

catch (error:SQLError)

{

}

]]&gt;

trace(&quot;Error message:&quot;, error.message); trace(&quot;Details:&quot;, error.details);

}

&lt;/mx:Script&gt;

&lt;/mx:WindowedApplication&gt;

Notice that in the openAsync() method call in the asynchronous example, and the open() method call in the synchronous example, the second argument is the constant SQLMode.UPDATE. Specifying SQLMode.UPDATE for the second parameter (openMode) causes the runtime to dispatch an error if the specified file doesn’t exist. If you pass SQLMode.CREATE for the openMode parameter (or if you leave the openMode parameter off), the runtime attempts to create a database file if the specified file doesn’t exist. However, if the file exists it is opened, which is the same as if you use SQLMode.Update. You can also specify SQLMode.READ for the openMode parameter to open an existing database in a read-only mode. In that case data can be retrieved from the database but no data can be added, deleted, or changed.

### Working with SQL statements {#working-with-sql-statements}

Adobe AIR 1.0 and later

An individual SQL statement (a query or command) is represented in the runtime as a SQLStatement object. Follow these steps to create and execute a SQL statement:

Create a SQLStatement instance.

The SQLStatement object represents the SQL statement in your application.

var selectData:SQLStatement = new SQLStatement();

Specify which database the query runs against.

To do this, set the SQLStatement object’s sqlConnection property to the SQLConnection instance that’s connected with the desired database.

// A SQLConnection named &quot;conn&quot; has been created previously selectData.sqlConnection = conn;

Specify the actual SQL statement.

Create the statement text as a String and assign it to the SQLStatement instance’s text property.

selectData.text = &quot;SELECT col1, col2 FROM my_table WHERE col1 = :param1&quot;;

Define functions to handle the result of the execute operation (asynchronous execution mode only).

Use the addEventListener() method to register functions as listeners for the SQLStatement instance’s result and

error events.

// using listener methods and addEventListener()

selectData.addEventListener(SQLEvent.RESULT, resultHandler); selectData.addEventListener(SQLErrorEvent.ERROR, errorHandler);

function resultHandler(event:SQLEvent):void

{

// do something after the statement execution succeeds

}

function errorHandler(event:SQLErrorEvent):void

{

// do something after the statement execution fails

}

Alternatively, you can specify listener methods using a Responder object. In that case you create the Responder instance and link the listener methods to it.

// using a Responder (flash.net.Responder)

var selectResponder = new Responder(onResult, onError); function onResult(result:SQLResult):void

{

// do something after the statement execution succeeds

}

function onError(error:SQLError):void

{

// do something after the statement execution fails

}

If the statement text includes parameter definitions, assign values for those parameters.

To assign parameter values, use the SQLStatement instance’s parameters associative array property.

selectData.parameters[&quot;:param1&quot;] = 25;

Execute the SQL statement.

Call the SQLStatement instance’s execute() method.

// using synchronous execution mode

// or listener methods in asynchronous execution mode selectData.execute();

Additionally, if you’re using a Responder instead of event listeners in asynchronous execution mode, pass the Responder instance to the execute() method.

// using a Responder in asynchronous execution mode selectData.execute(-1, selectResponder);

For specific examples that demonstrate these steps, see the following topics:

“Retrieving data from a database” on page 733

“Inserting data” on page 743

“Changing or deleting data” on page 749

### Using parameters in statements {#using-parameters-in-statements}

Adobe AIR 1.0 and later

Using SQL statement parameters allows you to create a reusable SQL statement. When you use statement parameters, values within the statement can change (such as values being added in an INSERT statement) but the basic statement text remains unchanged. Consequently, using parameters provides performance benefits and makes it easier to code an application.

**Understanding statement parameters**

Adobe AIR 1.0 and later

Frequently an application uses a single SQL statement multiple times in an application, with slight variation. For example, consider an inventory-tracking application where a user can add new inventory items to the database. The application code that adds an inventory item to the database executes a SQL INSERT statement that actually adds the data to the database. However, each time the statement is executed there is a slight variation. Specifically, the actual values that are inserted in the table are different because they are specific to the inventory item being added.

In cases where you have a SQL statement that’s used multiple times with different values in the statement, the best approach is to use a SQL statement that includes parameters rather than literal values in the SQL text. A parameter is a placeholder in the statement text that is replaced with an actual value each time the statement is executed. To use parameters in a SQL statement, you create the SQLStatement instance as usual. For the actual SQL statement assigned to the text property, use parameter placeholders rather than literal values. You then define the value for each parameter by setting the value of an element in the SQLStatement instance’s parameters property. The parameters property is an associative array, so you set a particular value using the following syntax:

statement.parameters[_parameter_identifier_] = _value_;

The _parameter_identifier_ is a string if you’re using a named parameter, or an integer index if you’re using an unnamed parameter.

Using named parameters

Adobe AIR 1.0 and later

A parameter can be a named parameter. A named parameter has a specific name that the database uses to match the parameter value to its placeholder location in the statement text. A parameter name consists of the “:” or “@” character followed by a name, as in the following examples:

:itemName

@firstName

The following code listing demonstrates the use of named parameters:

var sql:String =

&quot;INSERT INTO inventoryItems (name, productCode)&quot; + &quot;VALUES (:name, :productCode)&quot;;

var addItemStmt:SQLStatement = new SQLStatement(); addItemStmt.sqlConnection = conn;

addItemStmt.text = sql;

// set parameter values addItemStmt.parameters[&quot;:name&quot;] = &quot;Item name&quot;; addItemStmt.parameters[&quot;:productCode&quot;] = &quot;12345&quot;;

addItemStmt.execute();

Using unnamed parameters

Adobe AIR 1.0 and later

As an alternative to using named parameters, you can also use unnamed parameters. To use an unnamed parameter you denote a parameter in a SQL statement using a “?” character. Each parameter is assigned a numeric index, according to the order of the parameters in the statement, starting with index 0 for the first parameter. The following example demonstrates a version of the previous example, using unnamed parameters:

var sql:String =

&quot;INSERT INTO inventoryItems (name, productCode)&quot; + &quot;VALUES (?, ?)&quot;;

var addItemStmt:SQLStatement = new SQLStatement(); addItemStmt.sqlConnection = conn;

addItemStmt.text = sql;

// set parameter values addItemStmt.parameters[0] = &quot;Item name&quot;; addItemStmt.parameters[1] = &quot;12345&quot;;

addItemStmt.execute();

Benefits of using parameters

Adobe AIR 1.0 and later

Using parameters in a SQL statement provides several benefits:

**Better performance** A SQLStatement instance that uses parameters can execute more efficiently compared to one that dynamically creates the SQL text each time it executes. The performance improvement is because the statement is prepared a single time and can then be executed multiple times using different parameter values, without needing to recompile the SQL statement.

**Explicit data typing** Parameters are used to allow for typed substitution of values that are unknown at the time the SQL statement is constructed. The use of parameters is the only way to guarantee the storage class for a value passed in to the database. When parameters are not used, the runtime attempts to convert all values from their text representation to a storage class based on the associated column&#039;s type affinity.

For more information on storage classes and column affinity, see

“Data type support” on page 1125

.

**Greater security** The use of parameters helps prevent a malicious technique known as a SQL injection attack. In a SQL injection attack, a user enters SQL code in a user-accessible location (for example, a data entry field). If application code constructs a SQL statement by directly concatenating user input into the SQL text, the user-entered SQL code is executed against the database. The following listing shows an example of concatenating user input into SQL text. **Do not use this technique**:

// assume the variables &quot;username&quot; and &quot;password&quot;

// contain user-entered data

var sql:String = &quot;SELECT userId &quot; + &quot;FROM users &quot; +

&quot;WHERE username = &#039;&quot; + username + &quot;&#039; &quot; + &quot; AND password = &#039;&quot; + password + &quot;&#039;&quot;;

var statement:SQLStatement = new SQLStatement(); statement.text = sql;

Using statement parameters instead of concatenating user-entered values into a statement&#039;s text prevents a SQL injection attack. SQL injection can’t happen because the parameter values are treated explicitly as substituted values, rather than becoming part of the literal statement text. The following is the recommended alternative to the previous listing:

// assume the variables &quot;username&quot; and &quot;password&quot;

// contain user-entered data

var sql:String = &quot;SELECT userId &quot; + &quot;FROM users &quot; +

&quot;WHERE username = :username &quot; + &quot; AND password = :password&quot;;

var statement:SQLStatement = new SQLStatement(); statement.text = sql;

// set parameter values statement.parameters[&quot;:username&quot;] = username; statement.parameters[&quot;:password&quot;] = password;

### Retrieving data from a database {#retrieving-data-from-a-database}

Adobe AIR 1.0 and later

Retrieving data from a database involves two steps. First, you execute a SQL SELECT statement, describing the set of data you want from the database. Next, you access the retrieved data and display or manipulate it as needed by your application.

**Executing a SELECT statement**

Adobe AIR 1.0 and later

To retrieve existing data from a database, you use a SQLStatement instance. Assign the appropriate SQL SELECT

statement to the instance’s text property, then call its execute() method.

For details on the syntax of the SELECT statement, see

“SQL support in local databases” on page 1104

.

The following example demonstrates executing a SELECT statement to retrieve data from a table named “products,” using asynchronous execution mode:

var selectStmt:SQLStatement = new SQLStatement();

// A SQLConnection named &quot;conn&quot; has been created previously selectStmt.sqlConnection = conn;

selectStmt.text = &quot;SELECT itemId, itemName, price FROM products&quot;;

selectStmt.addEventListener(SQLEvent.RESULT, resultHandler); selectStmt.addEventListener(SQLErrorEvent.ERROR, errorHandler);

selectStmt.execute();

function resultHandler(event:SQLEvent):void

{

var result:SQLResult = selectStmt.getResult();

var numResults:int = result.data.length; for (var i:int = 0; i &lt; numResults; i++)

{

var row:Object = result.data[i];

var output:String = &quot;itemId: &quot; + row.itemId; output += &quot;; itemName: &quot; + row.itemName; output += &quot;; price: &quot; + row.price; trace(output);

}

}

function errorHandler(event:SQLErrorEvent):void

{

// Information about the error is available in the

// event.error property, which is an instance of

// the SQLError class.

}

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) creationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.data.SQLConnection; import flash.data.SQLResult; import flash.data.SQLStatement; import flash.errors.SQLError; import flash.events.SQLErrorEvent; import flash.events.SQLEvent;

private function init():void

{

var selectStmt:SQLStatement = new SQLStatement();

// A SQLConnection named &quot;conn&quot; has been created previously selectStmt.sqlConnection = conn;

selectStmt.text = &quot;SELECT itemId, itemName, price FROM products&quot;;

selectStmt.addEventListener(SQLEvent.RESULT, resultHandler); selectStmt.addEventListener(SQLErrorEvent.ERROR, errorHandler);

selectStmt.execute();

}

private function resultHandler(event:SQLEvent):void

{

var result:SQLResult = selectStmt.getResult();

var numResults:int = result.data.length; for (var i:int = 0; i &lt; numResults; i++)

{

var row:Object = result.data[i];

var output:String = &quot;itemId: &quot; + row.itemId; output += &quot;; itemName: &quot; + row.itemName; output += &quot;; price: &quot; + row.price; trace(output);

}

}

private function errorHandler(event:SQLErrorEvent):void

{

}

]]&gt;

// Information about the error is available in the

// event.error property, which is an instance of

// the SQLError class.

&lt;/mx:Script&gt;

&lt;/mx:WindowedApplication&gt;

The following example demonstrates executing a SELECT statement to retrieve data from a table named “products,” using synchronous execution mode:

var selectStmt:SQLStatement = new SQLStatement();

// A SQLConnection named &quot;conn&quot; has been created previously selectStmt.sqlConnection = conn;

selectStmt.text = &quot;SELECT itemId, itemName, price FROM products&quot;;

try

{

selectStmt.execute();

var result:SQLResult = selectStmt.getResult(); var numResults:int = result.data.length;

for (var i:int = 0; i &lt; numResults; i++)

{

var row:Object = result.data[i];

var output:String = &quot;itemId: &quot; + row.itemId; output += &quot;; itemName: &quot; + row.itemName; output += &quot;; price: &quot; + row.price; trace(output);

}

}

catch (error:SQLError)

{

// Information about the error is available in the

// error variable, which is an instance of

// the SQLError class.

}

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) creationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.data.SQLConnection; import flash.data.SQLResult; import flash.data.SQLStatement; import flash.errors.SQLError; import flash.events.SQLErrorEvent; import flash.events.SQLEvent;

private function init():void

{

var selectStmt:SQLStatement = new SQLStatement();

// A SQLConnection named &quot;conn&quot; has been created previously selectStmt.sqlConnection = conn;

selectStmt.text = &quot;SELECT itemId, itemName, price FROM products&quot;;

try

{

selectStmt.execute();

var result:SQLResult = selectStmt.getResult();

var numResults:int = result.data.length; for (var i:int = 0; i &lt; numResults; i++)

{

var row:Object = result.data[i];

var output:String = &quot;itemId: &quot; + row.itemId; output += &quot;; itemName: &quot; + row.itemName; output += &quot;; price: &quot; + row.price; trace(output);

}

}

catch (error:SQLError)

{

}

]]&gt;

// Information about the error is available in the

// error variable, which is an instance of

// the SQLError class.

}

&lt;/mx:Script&gt;

&lt;/mx:WindowedApplication&gt;

In asynchronous execution mode, when the statement finishes executing, the SQLStatement instance dispatches a result event (SQLEvent.RESULT) indicating that the statement was run successfully. Alternatively, if a Responder object is passed as an argument to the execute() method, the Responder object’s result handler function is called. In synchronous execution mode, execution pauses until the execute() operation completes, then continues on the next line of code.

Accessing SELECT statement result data

Adobe AIR 1.0 and later

Once the SELECT statement has finished executing, the next step is to access the data that was retrieved. You retrieve the result data from executing a SELECT statement by calling the SQLStatement object’s getResult() method:

var result:SQLResult = selectStatement.getResult();

The getResult() method returns a SQLResult object. The SQLResult object’s data property is an Array containing the results of the SELECT statement:

var numResults:int = result.data.length; for (var i:int = 0; i &lt; numResults; i++)

{

// row is an Object representing one row of result data var row:Object = result.data[i];

}

Each row of data in the SELECT result set becomes an Object instance contained in the data Array. That object has properties whose names match the result set’s column names. The properties contain the values from the result set’s columns. For example, suppose a SELECT statement specifies a result set with three columns named “itemId,” “itemName,” and “price.” For each row in the result set, an Object instance is created with properties named itemId, itemName, and price. Those properties contain the values from their respective columns.

The following code listing defines a SQLStatement instance whose text is a SELECT statement. The statement retrieves rows containing the firstName and lastName column values of all the rows of a table named employees. This example uses asynchronous execution mode. When the execution completes, the selectResult() method is called, and the resulting rows of data are accessed using SQLStatement.getResult() and displayed using the trace() method. Note that this listing assumes there is a SQLConnection instance named conn that has already been instantiated and is already connected to the database. It also assumes that the “employees” table has already been created and populated with data.

import flash.data.SQLConnection; import flash.data.SQLResult; import flash.data.SQLStatement; import flash.events.SQLErrorEvent; import flash.events.SQLEvent;

// ... create and open the SQLConnection instance named conn ...

// create the SQL statement

var selectStmt:SQLStatement = new SQLStatement(); selectStmt.sqlConnection = conn;

// define the SQL text var sql:String =

&quot;SELECT firstName, lastName &quot; + &quot;FROM employees&quot;;

selectStmt.text = sql;

// register listeners for the result and error events selectStmt.addEventListener(SQLEvent.RESULT, selectResult); selectStmt.addEventListener(SQLErrorEvent.ERROR, selectError);

// execute the statement selectStmt.execute();

function selectResult(event:SQLEvent):void

{

// access the result data

var result:SQLResult = selectStmt.getResult();

var numRows:int = result.data.length; for (var i:int = 0; i &lt; numRows; i++)

{

var output:String = &quot;&quot;;

for (var columnName:String in result.data[i])

{

output += columnName + &quot;: &quot; + result.data[i][columnName] + &quot;; &quot;;

}

trace(&quot;row[&quot; + i.toString() + &quot;]\t&quot;, output);

}

}

function selectError(event:SQLErrorEvent):void

{

trace(&quot;Error message:&quot;, event.error.message); trace(&quot;Details:&quot;, event.error.details);

}

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) creationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.data.SQLConnection; import flash.data.SQLResult; import flash.data.SQLStatement; import flash.events.SQLErrorEvent; import flash.events.SQLEvent;

private function init():void

{

// ... create and open the SQLConnection instance named conn ...

// create the SQL statement

var selectStmt:SQLStatement = new SQLStatement(); selectStmt.sqlConnection = conn;

// define the SQL text var sql:String =

&quot;SELECT firstName, lastName &quot; + &quot;FROM employees&quot;;

selectStmt.text = sql;

// register listeners for the result and error events selectStmt.addEventListener(SQLEvent.RESULT, selectResult); selectStmt.addEventListener(SQLErrorEvent.ERROR, selectError);

// execute the statement selectStmt.execute();

}

private function selectResult(event:SQLEvent):void

{

// access the result data

var result:SQLResult = selectStmt.getResult();

var numRows:int = result.data.length; for (var i:int = 0; i &lt; numRows; i++)

{

var output:String = &quot;&quot;;

for (var columnName:String in result.data[i])

{

output += columnName + &quot;: &quot; + result.data[i][columnName] + &quot;; &quot;;

}

trace(&quot;row[&quot; + i.toString() + &quot;]\t&quot;, output);

}

}

private function selectError(event:SQLErrorEvent):void

{

}

]]&gt;

trace(&quot;Error message:&quot;, event.error.message); trace(&quot;Details:&quot;, event.error.details);

&lt;/mx:Script&gt;

&lt;/mx:WindowedApplication&gt;

The following code listing demonstrates the same techniques as the preceding one, but uses synchronous execution mode. The example defines a SQLStatement instance whose text is a SELECT statement. The statement retrieves rows containing the firstName and lastName column values of all the rows of a table named employees. The resulting rows of data are accessed using SQLStatement.getResult() and displayed using the trace() method. Note that this listing assumes there is a SQLConnection instance named conn that has already been instantiated and is already connected to the database. It also assumes that the “employees” table has already been created and populated with data.

import flash.data.SQLConnection; import flash.data.SQLResult; import flash.data.SQLStatement; import flash.errors.SQLError;

// ... create and open the SQLConnection instance named conn ...

// create the SQL statement

var selectStmt:SQLStatement = new SQLStatement(); selectStmt.sqlConnection = conn;

// define the SQL text var sql:String =

&quot;SELECT firstName, lastName &quot; + &quot;FROM employees&quot;;

selectStmt.text = sql;

try

{

// execute the statement selectStmt.execute();

// access the result data

var result:SQLResult = selectStmt.getResult();

var numRows:int = result.data.length; for (var i:int = 0; i &lt; numRows; i++)

{

var output:String = &quot;&quot;;

for (var columnName:String in result.data[i])

{

output += columnName + &quot;: &quot; + result.data[i][columnName] + &quot;; &quot;;

}

trace(&quot;row[&quot; + i.toString() + &quot;]\t&quot;, output);

}

}

catch (error:SQLError)

{

trace(&quot;Error message:&quot;, error.message); trace(&quot;Details:&quot;, error.details);

}

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) creationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.data.SQLConnection; import flash.data.SQLResult; import flash.data.SQLStatement; import flash.errors.SQLError;

private function init():void

{

// ... create and open the SQLConnection instance named conn ...

// create the SQL statement

var selectStmt:SQLStatement = new SQLStatement(); selectStmt.sqlConnection = conn;

// define the SQL text var sql:String =

&quot;SELECT firstName, lastName &quot; + &quot;FROM employees&quot;;

selectStmt.text = sql;

try

{

// execute the statement selectStmt.execute();

// access the result data

var result:SQLResult = selectStmt.getResult();

var numRows:int = result.data.length; for (var i:int = 0; i &lt; numRows; i++)

{

var output:String = &quot;&quot;;

for (var columnName:String in result.data[i])

{

output += columnName + &quot;: &quot;;

output += result.data[i][columnName] + &quot;; &quot;;

}

trace(&quot;row[&quot; + i.toString() + &quot;]\t&quot;, output);

}

}

catch (error:SQLError)

{

}

]]&gt;

trace(&quot;Error message:&quot;, error.message); trace(&quot;Details:&quot;, error.details);

}

&lt;/mx:Script&gt;

&lt;/mx:WindowedApplication&gt;

Defining the data type of SELECT result data

Adobe AIR 1.0 and later

By default, each row returned by a SELECT statement is created as an Object instance with properties named for the result set&#039;s column names and with the value of each column as the value of its associated property. However, before executing a SQL SELECT statement, you can set the itemClass property of the SQLStatement instance to a class. By setting the itemClass property, each row returned by the SELECT statement is created as an instance of the designated class. The runtime assigns result column values to property values by matching the column names in the SELECT result set to the names of the properties in the itemClass class.

Any class assigned as an itemClass property value must have a constructor that does not require any parameters. In addition, the class must have a single property for each column returned by the SELECT statement. It is considered an error if a column in the SELECT list does not have a matching property name in the itemClass class.

Retrieving SELECT results in parts

Adobe AIR 1.0 and later

By default, a SELECT statement execution retrieves all the rows of the result set at one time. Once the statement completes, you usually process the retrieved data in some way, such as creating objects or displaying the data on the screen. If the statement returns a large number of rows, processing all the data at once can be demanding for the computer, which in turn will cause the user interface to not redraw itself.

You can improve the perceived performance of your application by instructing the runtime to return a specific number of result rows at a time. Doing so causes the initial result data to return more quickly. It also allows you to divide the result rows into sets, so that the user interface is updated after each set of rows is processed. Note that it’s only practical to use this technique in asynchronous execution mode.

To retrieve SELECT results in parts, specify a value for the SQLStatement.execute() method’s first parameter (the prefetch parameter). The prefetch parameter indicates the number of rows to retrieve the first time the statement executes. When you call a SQLStatement instance’s execute() method, specify a prefetch parameter value and only that many rows are retrieved:

var stmt:SQLStatement = new SQLStatement(); stmt.sqlConnection = conn;

stmt.text = &quot;SELECT ...&quot;; stmt.addEventListener(SQLEvent.RESULT, selectResult);

stmt.execute(20); // only the first 20 rows (or fewer) are returned

The statement dispatches the result event, indicating that the first set of result rows is available. The resulting SQLResult instance’s data property contains the rows of data, and its complete property indicates whether there are additional result rows to retrieve. To retrieve additional result rows, call the SQLStatement instance’s next() method. Like the execute() method, the next() method’s first parameter is used to indicate how many rows to retrieve the next time the result event is dispatched.

function selectResult(event:SQLEvent):void

{

var result:SQLResult = stmt.getResult(); if (result.data != null)

{

// ... loop through the rows or perform other processing ...

if (!result.complete)

{

stmt.next(20); // retrieve the next 20 rows

}

else

{

stmt.removeEventListener(SQLEvent.RESULT, selectResult);

}

}

}

The SQLStatement dispatches a result event each time the next() method returns a subsequent set of result rows. Consequently, the same listener function can be used to continue processing results (from next() calls) until all the rows are retrieved.

For more information, see the descriptions for the SQLStatement.execute() method (the prefetch parameter description) and the SQLStatement.next() method.

### Inserting data {#inserting-data}

Adobe AIR 1.0 and later

Adding data to a database involves executing a SQL INSERT statement. Once the statement has finished executing, you can access the primary key for the newly inserted row if the key was generated by the database.

**Executing an INSERT statement**

Adobe AIR 1.0 and later

To add data to a table in a database, you create and execute a SQLStatement instance whose text is a SQL INSERT

statement.

The following example uses a SQLStatement instance to add a row of data to the already-existing employees table. This example demonstrates inserting data using asynchronous execution mode. Note that this listing assumes that there is a SQLConnection instance named conn that has already been instantiated and is already connected to a database. It also assumes that the “employees” table has already been created.

import flash.data.SQLConnection; import flash.data.SQLResult; import flash.data.SQLStatement; import flash.events.SQLErrorEvent; import flash.events.SQLEvent;

// ... create and open the SQLConnection instance named conn ...

// create the SQL statement

var insertStmt:SQLStatement = new SQLStatement(); insertStmt.sqlConnection = conn;

// define the SQL text var sql:String =

&quot;INSERT INTO employees (firstName, lastName, salary) &quot; + &quot;VALUES (&#039;Bob&#039;, &#039;Smith&#039;, 8000)&quot;;

insertStmt.text = sql;

// register listeners for the result and failure (status) events insertStmt.addEventListener(SQLEvent.RESULT, insertResult); insertStmt.addEventListener(SQLErrorEvent.ERROR, insertError);

// execute the statement insertStmt.execute();

function insertResult(event:SQLEvent):void

{

trace(&quot;INSERT statement succeeded&quot;);

}

function insertError(event:SQLErrorEvent):void

{

trace(&quot;Error message:&quot;, event.error.message); trace(&quot;Details:&quot;, event.error.details);

}

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) creationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.data.SQLConnection; import flash.data.SQLResult; import flash.data.SQLStatement; import flash.events.SQLErrorEvent; import flash.events.SQLEvent;

private function init():void

{

// ... create and open the SQLConnection instance named conn ...

// create the SQL statement

var insertStmt:SQLStatement = new SQLStatement(); insertStmt.sqlConnection = conn;

// define the SQL text var sql:String =

&quot;INSERT INTO employees (firstName, lastName, salary) &quot; +

&quot;VALUES (&#039;Bob&#039;, &#039;Smith&#039;, 8000)&quot;;

insertStmt.text = sql;

// register listeners for the result and failure (status) events insertStmt.addEventListener(SQLEvent.RESULT, insertResult); insertStmt.addEventListener(SQLErrorEvent.ERROR, insertError);

// execute the statement insertStmt.execute();

}

private function insertResult(event:SQLEvent):void

{

trace(&quot;INSERT statement succeeded&quot;);

}

private function insertError(event:SQLErrorEvent):void

{

}

]]&gt;

trace(&quot;Error message:&quot;, event.error.message); trace(&quot;Details:&quot;, event.error.details);

&lt;/mx:Script&gt;

&lt;/mx:WindowedApplication&gt;

The following example adds a row of data to the already-existing employees table, using synchronous execution mode. Note that this listing assumes that there is a SQLConnection instance named conn that has already been instantiated and is already connected to a database. It also assumes that the “employees” table has already been created.

import flash.data.SQLConnection; import flash.data.SQLResult; import flash.data.SQLStatement; import flash.errors.SQLError;

// ... create and open the SQLConnection instance named conn ...

// create the SQL statement

var insertStmt:SQLStatement = new SQLStatement(); insertStmt.sqlConnection = conn;

// define the SQL text var sql:String =

&quot;INSERT INTO employees (firstName, lastName, salary) &quot; + &quot;VALUES (&#039;Bob&#039;, &#039;Smith&#039;, 8000)&quot;;

insertStmt.text = sql;

try

{

}

// execute the statement insertStmt.execute();

trace(&quot;INSERT statement succeeded&quot;);

catch (error:SQLError)

{

trace(&quot;Error message:&quot;, error.message); trace(&quot;Details:&quot;, error.details);

}

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) creationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.data.SQLConnection; import flash.data.SQLResult; import flash.data.SQLStatement; import flash.errors.SQLError;

private function init():void

{

// ... create and open the SQLConnection instance named conn ...

// create the SQL statement

var insertStmt:SQLStatement = new SQLStatement(); insertStmt.sqlConnection = conn;

// define the SQL text var sql:String =

&quot;INSERT INTO employees (firstName, lastName, salary) &quot; + &quot;VALUES (&#039;Bob&#039;, &#039;Smith&#039;, 8000)&quot;;

insertStmt.text = sql;

try

{

}

// execute the statement insertStmt.execute();

trace(&quot;INSERT statement succeeded&quot;);

catch (error:SQLError)

{

}

]]&gt;

trace(&quot;Error message:&quot;, error.message); trace(&quot;Details:&quot;, error.details);

}

&lt;/mx:Script&gt;

&lt;/mx:WindowedApplication&gt;

Retrieving a database-generated primary key of an inserted row

Adobe AIR 1.0 and later

Often after inserting a row of data into a table, your code needs to know a database-generated primary key or row identifier value for the newly inserted row. For example, once you insert a row in one table, you might want to add rows in a related table. In that case you would want to insert the primary key value as a foreign key in the related table. The primary key of a newly inserted row can be retrieved using the SQLResult object associated with the statement execution. This is the same object that’s used to access result data after a SELECT statement is executed. As with any SQL statement, when the execution of an INSERT statement completes the runtime creates a SQLResult instance. You access the SQLResult instance by calling the SQLStatement object’s getResult() method if you’re using an event listener or if you’re using synchronous execution mode. Alternatively, if you’re using asynchronous execution mode and you pass a Responder instance to the execute() call, the SQLResult instance is passed as an argument to the result handler function. In any case, the SQLResult instance has a property, lastInsertRowID, that contains the row identifier of the most-recently inserted row if the executed SQL statement is an INSERT statement.

The following example demonstrates accessing the primary key of an inserted row in asynchronous execution mode:

insertStmt.text = &quot;INSERT INTO ...&quot;; insertStmt.addEventListener(SQLEvent.RESULT, resultHandler); insertStmt.execute();

function resultHandler(event:SQLEvent):void

{

// get the primary key

var result:SQLResult = insertStmt.getResult();

var primaryKey:Number = result.lastInsertRowID;

// do something with the primary key

}

The following example demonstrates accessing the primary key of an inserted row in synchronous execution mode:

insertStmt.text = &quot;INSERT INTO ...&quot;;

try

{

}

insertStmt.execute();

// get the primary key

var result:SQLResult = insertStmt.getResult();

var primaryKey:Number = result.lastInsertRowID;

// do something with the primary key

catch (error:SQLError)

{

// respond to the error

}

Note that the row identifier may or may not be the value of the column that is designated as the primary key column in the table definition, according to the following rules:

*   If the table is defined with a primary key column whose affinity (column data type) is INTEGER, the lastInsertRowID property contains the value that was inserted into that row (or the value generated by the runtime if it’s an AUTOINCREMENT column).
*   If the table is defined with multiple primary key columns (a composite key) or with a single primary key column whose affinity is not INTEGER, behind the scenes the database generates an integer row identifier value for the row. That generated value is the value of the lastInsertRowID property.
*   The value is always the row identifier of the most-recently inserted row. If an INSERT statement causes a trigger to fire which in turn inserts a row, the lastInsertRowID property contains the row identifier of the last row inserted by the trigger rather than the row created by the INSERT statement.

As a consequence of these rules, if you want to have an explicitly defined primary key column whose value is available after an INSERT command through the SQLResult.lastInsertRowID property, the column must be defined as an INTEGER PRIMARY KEY column. Even if your table does not include an explicit INTEGER PRIMARY KEY column, it is equally acceptable to use the database-generated row identifier as a primary key for your table in the sense of defining relationships with related tables. The row identifier column value is available in any SQL statement by using one of the special column names ROWID, _ROWID_, or OID. You can create a foreign key column in a related table and use the row

identifier value as the foreign key column value just as you would with an explicitly declared INTEGER PRIMARY KEY column. In that sense, if you are using an arbitrary primary key rather than a natural key, and as long as you don’t mind the runtime generating the primary key value for you, it makes little difference whether you use an INTEGER PRIMARY KEY column or the system-generated row identifier as a table’s primary key for defining a foreign key relationship with between two tables.

For more information about primary keys and generated row identifiers, see

“SQL support in local databases” on

page 1104

.

### Changing or deleting data {#changing-or-deleting-data}

Adobe AIR 1.0 and later

The process for executing other data manipulation operations is identical to the process used to execute a SQL SELECT or INSERT statement, as described in

“Working with SQL statements” on page 729

. Simply substitute a different SQL statement in the SQLStatement instance’s text property:

*   To change existing data in a table, use an UPDATE statement.
*   To delete one or more rows of data from a table, use a DELETE statement.

For descriptions of these statements, see

“SQL support in local databases” on page 1104

.

### Working with multiple databases {#working-with-multiple-databases}

Adobe AIR 1.0 and later

Use the SQLConnection.attach() method to open a connection to an additional database on a SQLConnection instance that already has an open database. You give the attached database a name using the name parameter in the attach() method call. When writing statements to manipulate that database, you can then use that name in a prefix (using the form database-name.table-name) to qualify any table names in your SQL statements, indicating to the runtime that the table can be found in the named database.

You can execute a single SQL statement that includes tables from multiple databases that are connected to the same SQLConnection instance. If a transaction is created on the SQLConnection instance, that transaction applies to all SQL statements that are executed using the SQLConnection instance. This is true regardless of which attached database the statement runs on.

Alternatively, you can also create multiple SQLConnection instances in an application, each of which is connected to one or multiple databases. However, if you do use multiple connections to the same database keep in mind that a database transaction isn’t shared across SQLConnection instances. Consequently, if you connect to the same database file using multiple SQLConnection instances, you can’t rely on both connections’ data changes being applied in the expected manner. For example, if two UPDATE or DELETE statements are run against the same database through different SQLConnection instances, and an application error occurs after one operation takes place, the database data could be left in an intermediate state that would not be reversible and might affect the integrity of the database (and consequently the application).

### Handling database errors {#handling-database-errors}

Adobe AIR 1.0 and later

In general, database error handling is like other runtime error handling. You should write code that is prepared for errors that may occur, and respond to the errors rather than leave it up to the runtime to do so. In a general sense, the possible database errors can be divided into three categories: connection errors, SQL syntax errors, and constraint errors.

**Connection errors**

Adobe AIR 1.0 and later

Most database errors are connection errors, and they can occur during any operation. Although there are strategies for preventing connection errors, there is rarely a simple way to gracefully recover from a connection error if the database is a critical part of your application.

Most connection errors have to do with how the runtime interacts with the operating system, the file system, and the database file. For example, a connection error occurs if the user doesn’t have permission to create a database file in a particular location on the file system. The following strategies help to prevent connection errors:

**Use user-specific database files** Rather than using a single database file for all users who use the application on a single computer, give each user their own database file. The file should be located in a directory that’s associated with the user’s account. For example, it could be in the application’s storage directory, the user’s documents folder, the user’s desktop, and so forth.

**Consider different user types** Test your application with different types of user accounts, on different operating systems. Don’t assume that the user has administrator permission on the computer. Also, don’t assume that the individual who installed the application is the user who’s running the application.

**Consider various file locations** If you allow a user to specify where to save a database file or select a file to open, consider the possible file locations that the users might use. In addition, consider defining limits on where users can store (or from where they can open) database files. For example, you might only allow users to open files that are within their user account’s storage location.

If a connection error occurs, it most likely happens on the first attempt to create or open the database. This means that the user is unable to do any database-related operations in the application. For certain types of errors, such as read- only or permission errors, one possible recovery technique is to copy the database file to a different location. The application can copy the database file to a different location where the user does have permission to create and write to files, and use that location instead.

Syntax errors

Adobe AIR 1.0 and later

A syntax error occurs when a SQL statement is incorrectly formed, and the application attempts to execute the statement. Because local database SQL statements are created as strings, compile-time SQL syntax checking is not possible. All SQL statements must be executed to check their syntax. Use the following strategies to prevent SQL syntax errors:

**Test all SQL statements thoroughly** If possible, while developing your application test your SQL statements separately before encoding them as statement text in the application code. In addition, use a code-testing approach such as unit testing to create a set of tests that exercise every possible option and variation in the code.

**Use statement parameters and avoid concatenating (dynamically generating) SQL** Using parameters, and avoiding dynamically built SQL statements, means that the same SQL statement text is used each time a statement is executed. Consequently, it’s much easier to test your statements and limit the possible variation. If you must dynamically generate a SQL statement, keep the dynamic parts of the statement to a minimum. Also, carefully validate any user input to make sure it won’t cause syntax errors.

To recover from a syntax error, an application would need complex logic to be able to examine a SQL statement and correct its syntax. By following the previous guidelines for preventing syntax errors, your code can identify any potential run-time sources of SQL syntax errors (such as user input used in a statement). To recover from a syntax error, provide guidance to the user. Indicate what to correct to make the statement execute properly.

Constraint errors

Adobe AIR 1.0 and later

Constraint errors occur when an INSERT or UPDATE statement attempts to add data to a column. The error happens if the new data violates one of the defined constraints for the table or column. The set of possible constraints includes:

**Unique constraint** Indicates that across all the rows in a table, there cannot be duplicate values in one column. Alternatively, when multiple columns are combined in a unique constraint, the combination of values in those columns must not be duplicated. In other words, in terms of the specified unique column or columns, each row must be distinct.

**Primary key constraint** In terms of the data that a constraint allows and doesn’t allow, a primary key constraint is identical to a unique constraint.

**Not null constraint** Specifies that a single column cannot store a NULL value and consequently that in every row, that column must have a value.

**Check constraint** Allows you to specify an arbitrary constraint on one or more tables. A common check constraint is a rule that define that a column’s value must be within certain bounds (for example, that a numeric column’s value must be larger than 0). Another common type of check constraint specifies relationships between column values (for example, that a column’s value must be different from the value of another column in the same row).

**Data type (column affinity) constraint** The runtime enforces the data type of columns’ values, and an error occurs if an attempt is made to store a value of the incorrect type in a column. However, in many conditions values are converted to match the column’s declared data type. See

“Working with database data types” on page 752

for more information.

The runtime does not enforce constraints on foreign key values. In other words, foreign key values aren’t required to match an existing primary key value.

In addition to the predefined constraint types, the runtime SQL engine supports the use of triggers. A trigger is like an event handler. It is a predefined set of instructions that are carried out when a certain action happens. For example, a trigger could be defined that runs when data is inserted into or deleted from a particular table. One possible use of a trigger is to examine data changes and cause an error to occur if specified conditions aren’t met. Consequently, a trigger can serve the same purpose as a constraint, and the strategies for preventing and recovering from constraint errors also apply to trigger-generated errors. However, the error id for trigger-generated errors is different from the error id for constraint errors.

The set of constraints that apply to a particular table is determined while you’re designing an application. Consciously designing constraints makes it easier to design your application to prevent and recover from constraint errors.

However, constraint errors are difficult to systematically predict and prevent. Prediction is difficult because constraint errors don’t appear until application data is added. Constraint errors occur with data that is added to a database after

it’s created. These errors are often a result of the relationship between new data and data that already exists in the database. The following strategies can help you avoid many constraint errors:

**Carefully plan database structure and constraints** The purpose of constraints is to enforce application rules and help protect the integrity of the database’s data. When you’re planning your application, consider how to structure your database to support your application. As part of that process, identify rules for your data, such as whether certain values are required, whether a value has a default, whether duplicate values are allowed, and so forth. Those rules guide you in defining database constraints.

**Explicitly specify column names** An INSERT statement can be written without explicitly specifying the columns into which values are to be inserted, but doing so is an unnecessary risk. By explicitly naming the columns into which values are to be inserted, you can allow for automatically generated values, columns with default values, and columns that allow NULL values. In addition, by doing so you can ensure that all NOT NULL columns have an explicit value inserted.

**Use default values** Whenever you specify a NOT NULL constraint for a column, if at all possible specify a default value in the column definition. Application code can also provide default values. For example, your code can check if a String variable is null and assign it a value before using it to set a statement parameter value.

**Validate user-entered data** Check user-entered data ahead of time to make sure that it obeys limits specified by constraints, especially in the case of NOT NULL and CHECK constraints. Naturally, a UNIQUE constraint is more difficult to check for because doing so would require executing a SELECT query to determine whether the data is unique.

**Use triggers** You can write a trigger that validates (and possibly replaces) inserted data or takes other actions to correct invalid data. This validation and correction can prevent a constraint error from occurring.

In many ways constraint errors are more difficult to prevent than other types of errors. Fortunately, there are several strategies to recover from constraint errors in ways that don’t make the application unstable or unusable:

**Use conflict algorithms** When you define a constraint on a column, and when you create an INSERT or UPDATE statement, you have the option of specifying a conflict algorithm. A conflict algorithm defines the action the database takes when a constraint violation occurs. There are several possible actions the database engine can take. The database engine can end a single statement or a whole transaction. It can ignore the error. It can even remove old data and replace it with the data that the code is attempting to store.

For more information see the section “ON CONFLICT (conflict algorithms)” in the

“SQL support in local databases”

on page 1104

.

**Provide corrective feedback** The set of constraints that can affect a particular SQL command can be identified ahead of time. Consequently, you can anticipate constraint errors that a statement could cause. With that knowledge, you can build application logic to respond to a constraint error. For example, suppose an application includes a data entry form for entering new products. If the product name column in the database is defined with a UNIQUE constraint, the action of inserting a new product row in the database could cause a constraint error. Consequently, the application is designed to anticipate a constraint error. When the error happens, the application alerts the user, indicating that the specified product name is already in use and asking the user to choose a different name. Another possible response is to allow the user to view information about the other product with the same name.

### Working with database data types {#working-with-database-data-types}

Adobe AIR 1.0 and later

When a table is created in a database, the SQL statement for creating the table defines the affinity, or data type, for each column in the table. Although affinity declarations can be omitted, it’s a good idea to explicitly declare column affinity in your CREATE TABLE SQL statements.

As a general rule, any object that you store in a database using an INSERT statement is returned as an instance of the same data type when you execute a SELECT statement. However, the data type of the retrieved value can be different depending on the affinity of the database column in which the value is stored. When a value is stored in a column, if its data type doesn’t match the column’s affinity, the database attempts to convert the value to match the column’s affinity. For example, if a database column is declared with NUMERIC affinity, the database attempts to convert inserted data into a numeric storage class (INTEGER or REAL) before storing the data. The database throws an error if the data can’t be converted. According to this rule, if the String “12345” is inserted into a NUMERIC column, the database automatically converts it to the integer value 12345 before storing it in the database. When it’s retrieved with a SELECT statement, the value is returned as an instance of a numeric data type (such as Number) rather than as a String instance.

The best way to avoid undesirable data type conversion is to follow two rules. First, define each column with the affinity that matches the type of data that it is intended to store. Next, only insert values whose data type matches the defined affinity. Following these rules provides two benefits. When you insert the data it isn’t converted unexpectedly (possibly losing its intended meaning as a result). In addition, when you retrieve the data it is returned with its original data type.

For more information about the available column affinity types and using data types in SQL statements, see the

“Data

type support” on page 1125

.