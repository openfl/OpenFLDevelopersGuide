## Initializing XML variables {#initializing-xml-variables}

Flash Player 9 and later, Adobe AIR 1.0 and later

You can assign an XML literal to an XML object, as follows:

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

As the following snippet shows, you can also use the new constructor to create an instance of an XML object from a string that contains XML data:

var str:String = &quot;&lt;order&gt;&lt;item id=&#039;1&#039;&gt;&lt;menuName&gt;burger&lt;/menuName&gt;&quot;

+ &quot;&lt;price&gt;3.95&lt;/price&gt;&lt;/item&gt;&lt;/order&gt;&quot;; var myXML:XML = new XML(str);

If the XML data in the string is not well formed (for example, if a closing tag is missing), you will see a run-time error. You can also pass data by reference (from other variables) into an XML object, as the following example shows:

var tagname:String = &quot;item&quot;;

var attributename:String = &quot;id&quot;; var attributevalue:String = &quot;5&quot;; var content:String = &quot;Chicken&quot;;

var x:XML = &lt;{tagname} {attributename}={attributevalue}&gt;{content}&lt;/{tagname}&gt;; trace(x.toXMLString())

// Output: &lt;item id=&quot;5&quot;&gt;Chicken&lt;/item&gt;

To load XML data from a URL, use the URLLoader class, as the following example shows:

import flash.events.Event; import flash.net.URLLoader; import flash.net.URLRequest;

var externalXML:XML;

var loader:URLLoader = new URLLoader();

var request:URLRequest = new URLRequest(&quot;xmlFile.xml&quot;); loader.load(request); loader.addEventListener(Event.COMPLETE, onComplete);

function onComplete(event:Event):void

{

var loader:URLLoader = event.target as URLLoader; if (loader != null)

{

externalXML = new XML(loader.data); trace(externalXML.toXMLString());

}

else

{

trace(&quot;loader is not a URLLoader!&quot;);

}

}

To read XML data from a socket connection, use the XMLSocket class. For more information, see the [XMLSocket class](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/net/XMLSocket.html) in the [ActionScript 3.0 Reference for the Adobe Flash Platform](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/index.html).