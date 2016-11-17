## Multidimensional arrays {#multidimensional-arrays}

Flash Player 9 and later, Adobe AIR 1.0 and later

Multidimensional arrays contain other arrays as elements. For example, consider a list of tasks that is stored as an indexed array of strings:

var tasks:Array = [&quot;wash dishes&quot;, &quot;take out trash&quot;];

If you want to store a separate list of tasks for each day of the week, you can create a multidimensional array with one element for each day of the week. Each element contains an indexed array, similar to the tasks array, that stores the list of tasks. You can use any combination of indexed or associative arrays in multidimensional arrays. The examples in the following sections use either two indexed arrays or an associative array of indexed arrays. You might want to try the other combinations as exercises.

**Two indexed arrays**

Flash Player 9 and later, Adobe AIR 1.0 and later

When you use two indexed arrays, you can visualize the result as a table or spreadsheet. The elements of the first array represent the rows of the table, while the elements of the second array represent the columns.

For example, the following multidimensional array uses two indexed arrays to track task lists for each day of the week. The first array, masterTaskList, is created using the Array class constructor. Each element of the array represents a day of the week, with index 0 representing Monday, and index 6 representing Sunday. These elements can be thought of as the rows in the table. You can create each day’s task list by assigning an array literal to each of the seven elements that you create in the masterTaskList array. The array literals represent the columns in the table.

var masterTaskList:Array = new Array(); masterTaskList[0] = [&quot;wash dishes&quot;, &quot;take out trash&quot;]; masterTaskList[1] = [&quot;wash dishes&quot;, &quot;pay bills&quot;];

masterTaskList[2] = [&quot;wash dishes&quot;, &quot;dentist&quot;, &quot;wash dog&quot;]; masterTaskList[3] = [&quot;wash dishes&quot;];

masterTaskList[4] = [&quot;wash dishes&quot;, &quot;clean house&quot;]; masterTaskList[5] = [&quot;wash dishes&quot;, &quot;wash car&quot;, &quot;pay rent&quot;]; masterTaskList[6] = [&quot;mow lawn&quot;, &quot;fix chair&quot;];

You can access individual items on any of the task lists using the array access ([]) operator. The first set of brackets represents the day of the week, and the second set of brackets represents the task list for that day. For example, to retrieve the second task from Wednesday’s list, first use index 2 for Wednesday, and then use index 1 for the second task in the list.

trace(masterTaskList[2][1]); // output: dentist

To retrieve the first task from Sunday’s list, use index 6 for Sunday and index 0 for the first task on the list.

trace(masterTaskList[6][0]); // output: mow lawn

### Associative array with an indexed array {#associative-array-with-an-indexed-array}

Flash Player 9 and later, Adobe AIR 1.0 and later

To make the individual arrays easier to access, you can use an associative array for the days of the week and an indexed array for the task lists. Using an associative array allows you to use dot syntax when referring to a particular day of the week, but at the cost of extra run-time processing to access each element of the associative array. The following example uses an associative array as the basis of a task list, with a key and value pair for each day of the week:

var masterTaskList:Object = new Object(); masterTaskList[&quot;Monday&quot;] = [&quot;wash dishes&quot;, &quot;take out trash&quot;]; masterTaskList[&quot;Tuesday&quot;] = [&quot;wash dishes&quot;, &quot;pay bills&quot;];

masterTaskList[&quot;Wednesday&quot;] = [&quot;wash dishes&quot;, &quot;dentist&quot;, &quot;wash dog&quot;]; masterTaskList[&quot;Thursday&quot;] = [&quot;wash dishes&quot;]; masterTaskList[&quot;Friday&quot;] = [&quot;wash dishes&quot;, &quot;clean house&quot;]; masterTaskList[&quot;Saturday&quot;] = [&quot;wash dishes&quot;, &quot;wash car&quot;, &quot;pay rent&quot;]; masterTaskList[&quot;Sunday&quot;] = [&quot;mow lawn&quot;, &quot;fix chair&quot;];

Dot syntax makes the code more readable by making it possible to avoid multiple sets of brackets.

trace(masterTaskList.Wednesday[1]); // output: dentist trace(masterTaskList.Sunday[0]);// output: mow lawn

You can iterate through the task list using a for..in loop, but you must use the array access ([]) operator instead of dot syntax to access the value associated with each key. Because masterTaskList is an associative array, the elements are not necessarily retrieved in the order that you may expect, as the following example shows:

for (var day:String in masterTaskList)

{

trace(day + &quot;: &quot; + masterTaskList[day])

}

/* output:

Sunday: mow lawn,fix chair

Wednesday: wash dishes,dentist,wash dog Friday: wash dishes,clean house Thursday: wash dishes

Monday: wash dishes,take out trash Saturday: wash dishes,wash car,pay rent Tuesday: wash dishes,pay bills

*/