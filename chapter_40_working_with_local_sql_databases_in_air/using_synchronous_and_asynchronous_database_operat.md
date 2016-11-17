## Using synchronous and asynchronous database operations {#using-synchronous-and-asynchronous-database-operations}

Adobe AIR 1.0 and later

Previous sections have described common database operations such as retrieving, inserting, updating, and deleting data, as well as creating a database file and tables and other objects within a database. The examples have demonstrated how to perform these operations both asynchronously and synchronously.

As a reminder, in asynchronous execution mode, you instruct the database engine to perform an operation. The database engine then works in the background while the application keeps running. When the operation finishes the database engine dispatches an event to alert you to that fact. The key benefit of asynchronous execution is that the runtime performs the database operations in the background while the main application code continues executing. This is especially valuable when the operation takes a notable amount of time to run.

On the other hand, in synchronous execution mode operations don’t run in the background. You tell the database engine to perform an operation. The code pauses at that point while the database engine does its work. When the operation completes, execution continues with the next line of your code.

A single database connection can’t execute some operations or statements synchronously and others asynchronously. You specify whether a SQLConnection operates in synchronous or asynchronous when you open the connection to the database. If you call SQLConnection.open() the connection operates in synchronous execution mode, and if you call SQLConnection.openAsync() the connection operates in asynchronous execution mode. Once a SQLConnection instance is connected to a database using open() or openAsync(), it is fixed to synchronous or asynchronous execution.

**Using synchronous database operations**

Adobe AIR 1.0 and later

There is little difference in the actual code that you use to execute and respond to operations when using synchronous execution, compared to the code for asynchronous execution mode. The key differences between the two approaches fall into two areas. The first is executing an operation that depends on another operation (such as SELECT result rows or the primary key of the row added by an INSERT statement). The second area of difference is in handling errors.

**Writing code for synchronous operations**

Adobe AIR 1.0 and later

The key difference between synchronous and asynchronous execution is that in synchronous mode you write the code as a single series of steps. In contrast, in asynchronous code you register event listeners and often divide operations among listener methods. When a database is connected in synchronous execution mode, you can execute a series of database operations in succession within a single code block. The following example demonstrates this technique:

var conn:SQLConnection = new SQLConnection();

// The database file is in the application storage directory var folder:File = File.applicationStorageDirectory;

var dbFile:File = folder.resolvePath(&quot;DBSample.db&quot;);

// open the database conn.open(dbFile, OpenMode.UPDATE);

// start a transaction conn.begin();

// add the customer record to the database

var insertCustomer:SQLStatement = new SQLStatement(); insertCustomer.sqlConnection = conn; insertCustomer.text =

&quot;INSERT INTO customers (firstName, lastName) &quot; + &quot;VALUES (&#039;Bob&#039;, &#039;Jones&#039;)&quot;;

insertCustomer.execute();

var customerId:Number = insertCustomer.getResult().lastInsertRowID;

// add a related phone number record for the customer var insertPhoneNumber:SQLStatement = new SQLStatement(); insertPhoneNumber.sqlConnection = conn; insertPhoneNumber.text =

&quot;INSERT INTO customerPhoneNumbers (customerId, number) &quot; + &quot;VALUES (:customerId, &#039;800-555-1234&#039;)&quot;;

insertPhoneNumber.parameters[&quot;:customerId&quot;] = customerId; insertPhoneNumber.execute();

// commit the transaction conn.commit();

As you can see, you call the same methods to perform database operations whether you’re using synchronous or asynchronous execution. The key differences between the two approaches are executing an operation that depends on another operation and handling errors.

Executing an operation that depends on another operation

Adobe AIR 1.0 and later

When you’re using synchronous execution mode, you don’t need to write code that listens for an event to determine when an operation completes. Instead, you can assume that if an operation in one line of code completes successfully, execution continues with the next line of code. Consequently, to perform an operation that depends on the success of another operation, simply write the dependent code immediately following the operation on which it depends. For instance, to code an application to begin a transaction, execute an INSERT statement, retrieve the primary key of the inserted row, insert that primary key into another row of a different table, and finally commit the transaction, the code can all be written as a series of statements. The following example demonstrates these operations:

var conn:SQLConnection = new SQLConnection();

// The database file is in the application storage directory var folder:File = File.applicationStorageDirectory;

var dbFile:File = folder.resolvePath(&quot;DBSample.db&quot;);

// open the database conn.open(dbFile, SQLMode.UPDATE);

// start a transaction conn.begin();

// add the customer record to the database

var insertCustomer:SQLStatement = new SQLStatement(); insertCustomer.sqlConnection = conn; insertCustomer.text =

&quot;INSERT INTO customers (firstName, lastName) &quot; + &quot;VALUES (&#039;Bob&#039;, &#039;Jones&#039;)&quot;;

insertCustomer.execute();

var customerId:Number = insertCustomer.getResult().lastInsertRowID;

// add a related phone number record for the customer var insertPhoneNumber:SQLStatement = new SQLStatement(); insertPhoneNumber.sqlConnection = conn; insertPhoneNumber.text =

&quot;INSERT INTO customerPhoneNumbers (customerId, number) &quot; + &quot;VALUES (:customerId, &#039;800-555-1234&#039;)&quot;;

insertPhoneNumber.parameters[&quot;:customerId&quot;] = customerId; insertPhoneNumber.execute();

// commit the transaction conn.commit();

Handling errors with synchronous execution

Adobe AIR 1.0 and later

In synchronous execution mode, you don’t listen for an error event to determine that an operation has failed. Instead, you surround any code that could trigger errors in a set of try..catch..finally code blocks. You wrap the error- throwing code in the try block. Write the actions to perform in response to each type of error in separate catch blocks. Place any code that you want to always execute regardless of success or failure (for example, closing a database connection that’s no longer needed) in a finally block. The following example demonstrates using try..catch..finally blocks for error handling. It builds on the previous example by adding error handling code:

var conn:SQLConnection = new SQLConnection();

// The database file is in the application storage directory var folder:File = File.applicationStorageDirectory;

var dbFile:File = folder.resolvePath(&quot;DBSample.db&quot;);

// open the database conn.open(dbFile, SQLMode.UPDATE);

// start a transaction conn.begin();

try

{

}

// add the customer record to the database

var insertCustomer:SQLStatement = new SQLStatement(); insertCustomer.sqlConnection = conn; insertCustomer.text =

&quot;INSERT INTO customers (firstName, lastName)&quot; + &quot;VALUES (&#039;Bob&#039;, &#039;Jones&#039;)&quot;;

insertCustomer.execute();

var customerId:Number = insertCustomer.getResult().lastInsertRowID;

// add a related phone number record for the customer var insertPhoneNumber:SQLStatement = new SQLStatement(); insertPhoneNumber.sqlConnection = conn; insertPhoneNumber.text =

&quot;INSERT INTO customerPhoneNumbers (customerId, number)&quot; + &quot;VALUES (:customerId, &#039;800-555-1234&#039;)&quot;;

insertPhoneNumber.parameters[&quot;:customerId&quot;] = customerId; insertPhoneNumber.execute();

// if we&#039;ve gotten to this point without errors, commit the transaction conn.commit();

catch (error:SQLError)

{

// rollback the transaction conn.rollback();

}

### Understanding the asynchronous execution model {#understanding-the-asynchronous-execution-model}

Adobe AIR 1.0 and later

One common concern about using asynchronous execution mode is the assumption that you can’t start executing a SQLStatement instance if another SQLStatement is currently executing against the same database connection. In fact, this assumption isn’t correct. While a SQLStatement instance is executing you can’t change the text property of the statement. However, if you use a separate SQLStatement instance for each different SQL statement that you want to execute, you can call the execute() method of a SQLStatement while another SQLStatement instance is still executing, without causing an error.

Internally, when you’re executing database operations using asynchronous execution mode, each database connection (each SQLConnection instance) has its own queue or list of operations that it is instructed to perform. The runtime executes each operation in sequence, in the order they are added to the queue. When you create a SQLStatement instance and call its execute() method, that statement execution operation is added to the queue for the connection. If no operation is currently executing on that SQLConnection instance, the statement begins executing in the background. Suppose that within the same block of code you create another SQLStatement instance and also call that method’s execute() method. That second statement execution operation is added to the queue behind the first statement. As soon as the first statement finishes executing, the runtime moves to the next operation in the queue. The processing of subsequent operations in the queue happens in the background, even while the result event for the first operation is being dispatched in the main application code. The following code demonstrates this technique:

// Using asynchronous execution mode

var stmt1:SQLStatement = new SQLStatement(); stmt1.sqlConnection = conn;

// ... Set statement text and parameters, and register event listeners ... stmt1.execute();

// At this point stmt1&#039;s execute() operation is added to conn&#039;s execution queue.

var stmt2:SQLStatement = new SQLStatement(); stmt2.sqlConnection = conn;

// ... Set statement text and parameters, and register event listeners ... stmt2.execute();

// At this point stmt2&#039;s execute() operation is added to conn&#039;s execution queue.

// When stmt1 finishes executing, stmt2 will immediately begin executing

// in the background.

There is an important side effect of the database automatically executing subsequent queued statements. If a statement depends on the outcome of another operation, you can’t add the statement to the queue (in other words, you can’t call its execute() method) until the first operation completes. This is because once you’ve called the second statement’s execute() method, you can’t change the statement’s text or parameters properties. In that case you must wait for the event indicating that the first operation completes before starting the next operation. For example, if you want to execute a statement in the context of a transaction, the statement execution depends on the operation of opening the transaction. After calling the SQLConnection.begin() method to open the transaction, you need to wait for the SQLConnection instance to dispatch its begin event. Only then can you call the SQLStatement instance’s execute() method. In this example the simplest way to organize the application to ensure that the operations are executed properly is to create a method that’s registered as a listener for the begin event. The code to call the SQLStatement.execute() method is placed within that listener method.