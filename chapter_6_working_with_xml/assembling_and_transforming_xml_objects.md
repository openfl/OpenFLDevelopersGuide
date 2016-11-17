## Assembling and transforming XML objects {#assembling-and-transforming-xml-objects}

Flash Player 9 and later, Adobe AIR 1.0 and later

Use the prependChild() method or the appendChild() method to add a property to the beginning or end of an XML object’s list of properties, as the following example shows:

var x1:XML = &lt;p&gt;Line 1&lt;/p&gt; var x2:XML = &lt;p&gt;Line 2&lt;/p&gt; var x:XML = &lt;body&gt;&lt;/body&gt; x = x.appendChild(x1);

x = x.appendChild(x2);

x = x.prependChild(&lt;p&gt;Line 0&lt;/p&gt;);

// x == &lt;body&gt;&lt;p&gt;Line 0&lt;/p&gt;&lt;p&gt;Line 1&lt;/p&gt;&lt;p&gt;Line 2&lt;/p&gt;&lt;/body&gt;

Use the insertChildBefore() method or the insertChildAfter() method to add a property before or after a specified property, as follows:

var x:XML =

&lt;body&gt;

&lt;p&gt;Paragraph 1&lt;/p&gt;

&lt;p&gt;Paragraph 2&lt;/p&gt;

&lt;/body&gt;

var newNode:XML = &lt;p&gt;Paragraph 1.5&lt;/p&gt; x = x.insertChildAfter(x.p[0], newNode)

x = x.insertChildBefore(x.p[2], &lt;p&gt;Paragraph 1.75&lt;/p&gt;)

As the following example shows, you can also use curly brace operators ( { and } ) to pass data by reference (from other variables) when constructing XML objects:

var ids:Array = [121, 122, 123];

var names:Array = [[&quot;Murphy&quot;,&quot;Pat&quot;], [&quot;Thibaut&quot;,&quot;Jean&quot;], [&quot;Smith&quot;,&quot;Vijay&quot;]] var x:XML = new XML(&quot;&lt;employeeList&gt;&lt;/employeeList&gt;&quot;);

for (var i:int = 0; i &lt; 3; i++)

{

var newnode:XML = new XML(); newnode =

&lt;employee id={ids[i]}&gt;

&lt;last&gt;{names[i][0]}&lt;/last&gt;

&lt;first&gt;{names[i][1]}&lt;/first&gt;

&lt;/employee&gt;;

x = x.appendChild(newnode)

}

You can assign properties and attributes to an XML object by using the = operator, as in the following:

var x:XML =

&lt;employee&gt;

&lt;lastname&gt;Smith&lt;/lastname&gt;

&lt;/employee&gt; x.firstname = &quot;Jean&quot;; x.@id = &quot;239&quot;;

This sets the XML object x to the following:

&lt;employee id=&quot;239&quot;&gt;

&lt;lastname&gt;Smith&lt;/lastname&gt;

&lt;firstname&gt;Jean&lt;/firstname&gt;

&lt;/employee&gt;

You can use the + and += operators to concatenate XMLList objects:

var x1:XML = &lt;a&gt;test1&lt;/a&gt; var x2:XML = &lt;b&gt;test2&lt;/b&gt; var xList:XMLList = x1 + x2; xList += &lt;c&gt;test3&lt;/c&gt;

This sets the XMLList object xList to the following:

&lt;a&gt;test1&lt;/a&gt;

&lt;b&gt;test2&lt;/b&gt;

&lt;c&gt;test3&lt;/c&gt;