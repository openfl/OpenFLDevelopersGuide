## Using XML namespaces {#using-xml-namespaces}

Flash Player 9 and later, Adobe AIR 1.0 and later

Namespaces in an XML object (or document) identify the type of data that the object contains. For example, in sending and delivering XML data to a web service that uses the SOAP messaging protocol, you declare the namespace in the opening tag of the XML:

var message:XML =

&lt;soap:Envelope [xmlns:soap=&quot;http://schemas.xmlsoap.org/soap/envelope/&quot;](http://schemas.xmlsoap.org/soap/envelope/) [soap:encodingStyle=&quot;http://schemas.xmlsoap.org/soap/encoding/&quot;&gt;](http://schemas.xmlsoap.org/soap/encoding/)

&lt;soap:Body [xmlns:w=&quot;http://www.test.com/weather/&quot;&gt;](http://www.test.com/weather/)

&lt;w:getWeatherResponse&gt;

&lt;w:tempurature &gt;78&lt;/w:tempurature&gt;

&lt;/w:getWeatherResponse&gt;

&lt;/soap:Body&gt;

&lt;/soap:Envelope&gt;;

The namespace has a prefix, soap, and a URI that defines the namespace,

[http://schemas.xmlsoap.org/soap/envelope/.](http://schemas.xmlsoap.org/soap/envelope/)

ActionScript 3.0 includes the Namespace class for working with XML namespaces. For the XML object in the previous example, you can use the Namespace class as follows:

var soapNS:Namespace = message.namespace(&quot;soap&quot;);

trace(soapNS); // Output: [http://schemas.xmlsoap.org/soap/envelope/](http://schemas.xmlsoap.org/soap/envelope/)

var wNS:Namespace = new Namespace(&quot;w&quot;, [&quot;http://www.test.com/weather/&quot;);](http://www.test.com/weather/) message.addNamespace(wNS);

var encodingStyle:XMLList = message.@soapNS::encodingStyle; var body:XMLList = message.soapNS::Body;

message.soapNS::Body.wNS::GetWeatherResponse.wNS::tempurature = &quot;78&quot;;

The XML class includes the following methods for working with namespaces: addNamespace(), inScopeNamespaces(), localName(), name(), namespace(), namespaceDeclarations(), removeNamespace(), setLocalName(), setName(), and setNamespace().

The default xml namespace directive lets you assign a default namespace for XML objects. For example, in the following, both x1 and x2 have the same default namespace:

var ns1:Namespace = new [Namespace(&quot;http://www.example.com/namespaces/&quot;);](http://www.example.com/namespaces/) default xml namespace = ns1;

var x1:XML = &lt;test1 /&gt;; var x2:XML = &lt;test2 /&gt;;