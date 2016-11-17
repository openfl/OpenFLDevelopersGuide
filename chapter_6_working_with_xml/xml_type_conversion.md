## XML type conversion {#xml-type-conversion}

Flash Player 9 and later, Adobe AIR 1.0 and later

You can convert XML objects and XMLList objects to String values. Similarly, you can convert strings to XML objects and XMLList objects. Also, keep in mind that all XML attribute values, names, and text values are strings. The following sections discuss all these forms of XML type conversion.

**Converting XML and XMLList objects to strings**

Flash Player 9 and later, Adobe AIR 1.0 and later

The XML and XMLList classes include a toString() method and a toXMLString() method. The toXMLString() method returns a string that includes all tags, attributes, namespace declarations, and content of the XML object. For XML objects with complex content (child elements), the toString() method does exactly the same as the toXMLString() method. For XML objects with simple content (those that contain only one text element), the toString() method returns only the text content of the element, as the following example shows:

var myXML:XML =

&lt;order&gt;

&lt;item id=&#039;1&#039; quantity=&#039;2&#039;&gt;

&lt;menuName&gt;burger&lt;/menuName&gt;

&lt;price&gt;3.95&lt;/price&gt;

&lt;/item&gt;

&lt;order&gt;;

trace(myXML.item[0].menuName.toXMLString());

// &lt;menuName&gt;burger&lt;/menuName&gt; trace(myXML.item[0].menuName.toString());

// burger

If you use the trace() method without specifying toString() or toXMLString(), the data is converted using the

toString() method by default, as this code shows:

var myXML:XML =

&lt;order&gt;

&lt;item id=&#039;1&#039; quantity=&#039;2&#039;&gt;

&lt;menuName&gt;burger&lt;/menuName&gt;

&lt;price&gt;3.95&lt;/price&gt;

&lt;/item&gt;

&lt;order&gt;;

trace(myXML.item[0].menuName);

// burger

When using the trace() method to debug code, you will often want to use the toXMLString() method so that the

trace() method outputs more complete data.

### Converting strings to XML objects {#converting-strings-to-xml-objects}

Flash Player 9 and later, Adobe AIR 1.0 and later

You can use the new XML() constructor to create an XML object from a string, as follows:

var x:XML = new XML(&quot;&lt;a&gt;test&lt;/a&gt;&quot;);

If you attempt to convert a string to XML from a string that represents invalid XML or XML that is not well formed, a run-time error is thrown, as follows:

var x:XML = new XML(&quot;&lt;a&gt;test&quot;); // throws an error

### Converting attribute values, names, and text values from strings {#converting-attribute-values-names-and-text-values-from-strings}

Flash Player 9 and later, Adobe AIR 1.0 and later

All XML attribute values, names, and text values are String data types, and you may need to convert these to other data types. For example, the following code uses the Number() function to convert text values to numbers:

var myXML:XML =

&lt;order&gt;

&lt;item&gt;

&lt;price&gt;3.95&lt;/price&gt;

&lt;/item&gt;

&lt;item&gt;

&lt;price&gt;1.00&lt;/price&gt;

&lt;/item&gt;

&lt;/order&gt;;

var total:XML = &lt;total&gt;0&lt;/total&gt;; myXML.appendChild(total);

for each (var item:XML in myXML.item)

{

myXML.total.children()[0] = Number(myXML.total.children()[0])

+ Number(item.price.children()[0]);

}

trace(myXML.total); // 4.95;

If this code did not use the Number() function, the code would interpret the + operator as the string concatenation operator, and the trace() method in the last line would output the following:

01.003.95