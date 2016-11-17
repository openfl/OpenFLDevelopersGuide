## Traversing XML structures {#traversing-xml-structures}

Flash Player 9 and later, Adobe AIR 1.0 and later

One of the powerful features of XML is its ability to provide complex, nested data via a linear string of text characters. When you load data into an XML object, ActionScript parses the data and loads its hierarchical structure into memory (or it sends a run-time error if the XML data is not well formed).

The operators and methods of the XML and XMLList objects make it easy to traverse the structure of XML data.

Use the dot (.) operator and the descendent accessor (..) operator to access child properties of an XML object. Consider the following XML object:

var myXML:XML =

&lt;order&gt;

&lt;book ISBN=&quot;0942407296&quot;&gt;

&lt;title&gt;Baking Extravagant Pastries with Kumquats&lt;/title&gt;

&lt;author&gt;

&lt;lastName&gt;Contino&lt;/lastName&gt;

&lt;firstName&gt;Chuck&lt;/firstName&gt;

&lt;/author&gt;

&lt;pageCount&gt;238&lt;/pageCount&gt;

&lt;/book&gt;

&lt;book ISBN=&quot;0865436401&quot;&gt;

&lt;title&gt;Emu Care and Breeding&lt;/title&gt;

&lt;editor&gt;

&lt;lastName&gt;Case&lt;/lastName&gt;

&lt;firstName&gt;Justin&lt;/firstName&gt;

&lt;/editor&gt;

&lt;pageCount&gt;115&lt;/pageCount&gt;

&lt;/book&gt;

&lt;/order&gt;

The object myXML.book is an XMLList object containing child properties of the myXML object that have the name book. These are two XML objects, matching the two book properties of the myXML object.

The object myXML..lastName is an XMLList object containing any descendent properties with the name lastName. These are two XML objects, matching the two lastName of the myXML object.

The object myXML.book.editor.lastName is an XMLList object containing any children with the name lastName of children with the name editor of children with the name book of the myXML object: in this case, an XMLList object containing only one XML object (the lastName property with the value &quot;Case&quot;).

**Accessing parent and child nodes**

Flash Player 9 and later, Adobe AIR 1.0 and later

The parent() method returns the parent of an XML object.

You can use the ordinal index values of a child list to access specific child objects. For example, consider an XML object myXML that has two child properties named book. Each child property named book has an index number associated with it:

myXML.book[0] myXML.book[1]

To access a specific grandchild, you can specify index numbers for both the child and grandchild names:

myXML.book[0].title[0]

However, if there is only one child of x.book[0] that has the name title, you can omit the index reference, as follows:

myXML.book[0].title

Similarly, if there is only one book child of the object x, and if that child object has only one title object, you can omit both index references, like this:

myXML.book.title

You can use the child() method to navigate to children with names based on a variable or expression, as the following example shows:

var myXML:XML =

&lt;order&gt;

&lt;book&gt;

&lt;title&gt;Dictionary&lt;/title&gt;

&lt;/book&gt;

&lt;/order&gt;;

var childName:String = &quot;book&quot;; trace(myXML.child(childName).title) // output: Dictionary

### Accessing attributes {#accessing-attributes}

Flash Player 9 and later, Adobe AIR 1.0 and later

Use the @ symbol (the attribute identifier operator) to access attributes in an XML or XMLList object, as shown in the following code:

var employee:XML =

&lt;employee id=&quot;6401&quot; code=&quot;233&quot;&gt;

&lt;lastName&gt;Wu&lt;/lastName&gt;

&lt;firstName&gt;Erin&lt;/firstName&gt;

&lt;/employee&gt;; trace(employee.@id); // 6401

You can use the * wildcard symbol with the @ symbol to access all attributes of an XML or XMLList object, as in the following code:

var employee:XML =

&lt;employee id=&quot;6401&quot; code=&quot;233&quot;&gt;

&lt;lastName&gt;Wu&lt;/lastName&gt;

&lt;firstName&gt;Erin&lt;/firstName&gt;

&lt;/employee&gt;; trace(employee.@*.toXMLString());

// 6401

// 233

You can use the attribute() or attributes() method to access a specific attribute or all attributes of an XML or XMLList object, as in the following code:

var employee:XML =

&lt;employee id=&quot;6401&quot; code=&quot;233&quot;&gt;

&lt;lastName&gt;Wu&lt;/lastName&gt;

&lt;firstName&gt;Erin&lt;/firstName&gt;

&lt;/employee&gt;; trace(employee.attribute(&quot;id&quot;)); // 6401 trace(employee.attribute(&quot;*&quot;).toXMLString());

// 6401

// 233 trace(employee.attributes().toXMLString());

// 6401

// 233

Note that you can also use the following syntax to access attributes, as the following example shows:

employee.attribute(&quot;id&quot;) employee[&quot;@id&quot;] employee.@[&quot;id&quot;]

These are each equivalent to employee.@id. However, the syntax employee.@id is the preferred approach.

### Filtering by attribute or element value {#filtering-by-attribute-or-element-value}

Flash Player 9 and later, Adobe AIR 1.0 and later

You can use the parentheses operators— ( and ) —to filter elements with a specific element name or attribute value. Consider the following XML object:

var x:XML =

&lt;employeeList&gt;

&lt;employee id=&quot;347&quot;&gt;

&lt;lastName&gt;Zmed&lt;/lastName&gt;

&lt;firstName&gt;Sue&lt;/firstName&gt;

&lt;position&gt;Data analyst&lt;/position&gt;

&lt;/employee&gt;

&lt;employee id=&quot;348&quot;&gt;

&lt;lastName&gt;McGee&lt;/lastName&gt;

&lt;firstName&gt;Chuck&lt;/firstName&gt;

&lt;position&gt;Jr. data analyst&lt;/position&gt;

&lt;/employee&gt;

&lt;/employeeList&gt;

The following expressions are all valid:

*   x.employee.(lastName == &quot;McGee&quot;)—This is the second employee node.
*   x.employee.(lastName == &quot;McGee&quot;).firstName—This is the firstName property of the second employee node.
*   x.employee.(lastName == &quot;McGee&quot;).@id—This is the value of the id attribute of the second employee node.
*   x.employee.(@id == 347)—The first employee node.
*   x.employee.(@id == 347).lastName—This is the lastName property of the first employee node.
*   x.employee.(@id &gt; 300)—This is an XMLList with both employee properties.
*   x.employee.(position.toString().search(&quot;analyst&quot;) &gt; -1)—This is an XMLList with both position

properties.

If you try to filter on attributes or elements that do not exist, an exception is thrown. For example, the final line of the following code generates an error, because there is no id attribute in the second p element:

var doc:XML =

&lt;body&gt;

&lt;p id=&#039;123&#039;&gt;Hello, &lt;b&gt;Bob&lt;/b&gt;.&lt;/p&gt;

&lt;p&gt;Hello.&lt;/p&gt;

&lt;/body&gt;; trace(doc.p.(@id == &#039;123&#039;));

Similarly, the final line of following code generates an error because there is no b property of the second p element:

var doc:XML =

&lt;body&gt;

&lt;p id=&#039;123&#039;&gt;Hello, &lt;b&gt;Bob&lt;/b&gt;.&lt;/p&gt;

&lt;p&gt;Hello.&lt;/p&gt;

&lt;/body&gt;; trace(doc.p.(b == &#039;Bob&#039;));

To avoid these errors, you can identify the properties that have the matching attributes or elements by using the

attribute() and elements() methods, as in the following code:

var doc:XML =

&lt;body&gt;

&lt;p id=&#039;123&#039;&gt;Hello, &lt;b&gt;Bob&lt;/b&gt;.&lt;/p&gt;

&lt;p&gt;Hello.&lt;/p&gt;

&lt;/body&gt;; trace(doc.p.(attribute(&#039;id&#039;) == &#039;123&#039;));

trace(doc.p.(elements(&#039;b&#039;) == &#039;Bob&#039;));

You can also use the hasOwnProperty() method, as in the following code:

var doc:XML =

&lt;body&gt;

&lt;p id=&#039;123&#039;&gt;Hello, &lt;b&gt;Bob&lt;/b&gt;.&lt;/p&gt;

&lt;p&gt;Hello.&lt;/p&gt;

&lt;/body&gt;; trace(doc.p.(hasOwnProperty(&#039;@id&#039;) &amp;&amp; @id == &#039;123&#039;)); trace(doc.p.(hasOwnProperty(&#039;b&#039;) &amp;&amp; b == &#039;Bob&#039;));

### Using the for..in and the for each..in statements {#using-the-for-in-and-the-for-each-in-statements}

Flash Player 9 and later, Adobe AIR 1.0 and later

ActionScript 3.0 includes the for..in statement and the for each..in statement for iterating through XMLList objects. For example, consider the following XML object, myXML, and the XMLList object, myXML.item. The XMLList object, myXML.item, consists of the two item nodes of the XML object.

var myXML:XML =

&lt;order&gt;

&lt;item id=&#039;1&#039; quantity=&#039;2&#039;&gt;

&lt;menuName&gt;burger&lt;/menuName&gt;

&lt;price&gt;3.95&lt;/price&gt;

&lt;/item&gt;

&lt;item id=&#039;2&#039; quantity=&#039;2&#039;&gt;

&lt;menuName&gt;fries&lt;/menuName&gt;

&lt;price&gt;1.45&lt;/price&gt;

&lt;/item&gt;

&lt;/order&gt;;

The for..in statement lets you iterate over a set of property names in an XMLList:

var total:Number = 0;

for (var pname:String in myXML.item)

{

total += myXML.item.@quantity[pname] * myXML.item.price[pname];

}

The for each..in statement lets you iterate through the properties in the XMLList:

var total2:Number = 0;

for each (var prop:XML in myXML.item)

{

total2 += prop.@quantity * prop.price;

}