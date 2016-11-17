## The E4X approach to XML processing {#the-e4x-approach-to-xml-processing}

Flash Player 9 and later, Adobe AIR 1.0 and later

The ECMAScript for XML specification defines a set of classes and functionality for working with XML data. These classes and functionality are known collectively as _E4X._ ActionScript 3.0 includes the following E4X classes: XML, XMLList, QName, and Namespace.

The methods, properties, and operators of the E4X classes are designed with the following goals:

*   Simplicity—Where possible, E4X makes it easier to write and understand code for working with XML data.
*   Consistency—The methods and reasoning behind E4X are internally consistent and consistent with other parts of ActionScript.
*   Familiarity—You manipulate XML data with well-known operators, such as the dot (.) operator.

**_Note:_ **_There is a different XML class in ActionScript 2.0\. In ActionScript 3.0 that class has been renamed as XMLDocument, so that the name does not conflict with the ActionScript 3.0 XML class that is part of E4X. In ActionScript 3.0, the legacy classes—XMLDocument, XMLNode, XMLParser, and XMLTag—are included in the flash.xml package primarily for legacy support. The new E4X classes are core classes; you need not import a package to use them. For details on the legacy ActionScript 2.0 XML classes, see the_ [_flash.xml package_](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/xml/package-detail.html)_in the_ [_ActionScript 3.0_](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/index.html)[_Reference for the Adobe Flash Platform_](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/index.html)_._

Here is an example of manipulating data with E4X:

var myXML:XML =

&lt;order&gt;

&lt;item id=&#039;1&#039;&gt;

&lt;menuName&gt;burger&lt;/menuName&gt;

&lt;price&gt;3.95&lt;/price&gt;

&lt;/item&gt;

&lt;item id=&#039;2&#039;&gt;

&lt;menuName&gt;fries&lt;/menuName&gt;

&lt;price&gt;1.45&lt;/price&gt;

&lt;/item&gt;

&lt;/order&gt;

Often, your application will load XML data from an external source, such as a web service or a RSS feed. However, for clarity, the code examples provided here assign XML data as literals.

As the following code shows, E4X includes some intuitive operators, such as the dot (.) and attribute identifier (@) operators, for accessing properties and attributes in the XML:

trace(myXML.item[0].menuName); // Output: burger trace(myXML.item.(@id==2).menuName); // Output: fries trace(myXML.item.(menuName==&quot;burger&quot;).price); // Output: 3.95

Use the appendChild() method to assign a new child node to the XML, as the following snippet shows:

var newItem:XML =

&lt;item id=&quot;3&quot;&gt;

&lt;menuName&gt;medium cola&lt;/menuName&gt;

&lt;price&gt;1.25&lt;/price&gt;

&lt;/item&gt;

myXML.appendChild(newItem);

Use the @ and . operators not only to read data, but also to assign data, as in the following:

myXML.item[0].menuName=&quot;regular burger&quot;; myXML.item[1].menuName=&quot;small fries&quot;; myXML.item[2].menuName=&quot;medium cola&quot;;

myXML.item.(menuName==&quot;regular burger&quot;).@quantity = &quot;2&quot;; myXML.item.(menuName==&quot;small fries&quot;).@quantity = &quot;2&quot;; myXML.item.(menuName==&quot;medium cola&quot;).@quantity = &quot;2&quot;;

Use a for loop to iterate through nodes of the XML, as follows:

var total:Number = 0;

for each (var property:XML in myXML.item)

{

var q:int = Number(property.@quantity); var p:Number = Number(property.price); var itemTotal:Number = q * p;

total += itemTotal;

trace(q + &quot; &quot; + property.menuName + &quot; $&quot; + itemTotal.toFixed(2))

}

trace(&quot;Total: $&quot;, total.toFixed(2));