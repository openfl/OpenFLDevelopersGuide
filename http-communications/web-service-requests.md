# Web service requests {#web-service-requests}

There are a variety of HTTP-based web services. The main types include:

*   REST
*   XML-RPC
*   SOAP

To use a web service in Haxe 3, you create a URLRequest object, construct the web service call using either URL variables or an XML document, and send the call to the service using a URLLoader object. The Flex framework contains several classes that make it easier to use web services—especially useful when accessing complex SOAP services. Starting with Flash Professional CS3, you can use the Flex classes in applications developed with Flash Professional as well as in applications developed in Flash Builder.

In HTML-based AIR applications, you can use either the URLRequest and URLLoader classes or the JavaScript XMLHttpRequest class. If desired, you can also create a SWF library that exposes the web service components of the Flex framework to your JavaScript code.

When your application runs in a browser, you can only use web services in the same Internet domain as the calling SWF unless the server hosting the web service also hosts a cross-domain policy file that permits access from other domains. A technique that is often used when a cross-domain policy file is not available is to proxy the requests through your own server. Adobe Blaze DS and Adobe LiveCycle support web service proxying.

In AIR applications, a cross-domain policy file is not required when the web service call originates from the application security sandbox. AIR application content is never served from a remote domain, so it cannot participate in the types of attacks that cross-domain policies prevent. In HTML-based AIR applications, content in the application security sandbox can make cross-domain XMLHttpRequests. You can allow content in other security sandboxes to make cross- domain XMLHttpRequests as long as that content is loaded into an iframe.

## More Help topics

"Website controls (policy files)" on page 1051

[Adobe BlazeDS](http://opensource.adobe.com/wiki/display/blazeds/BlazeDS)

[Adobe LiveCycle ES2](http://www.adobe.com/devnet/livecycle/) [REST architecture](http://www.ics.uci.edu/%7Efielding/pubs/dissertation/rest_arch_style.htm) [XML-RPC](http://en.wikipedia.org/wiki/XML-RPC)

[SOAP protocol](http://www.w3.org/TR/soap/)

**REST-style web service requests**

REST-style web services use HTTP method verbs to designate the basic action and URL variables to specify the action details. For example, a request to get data for an item could use the GET verb and URL variables to specify a method name and item ID. The resulting URL string might look like:

[http://service.example.com/?method=getItem&amp;id=d3452](http://service.example.com/?method=getItem&amp;id=d3452)

To access a REST-style web service with Haxe, you can use the URLRequest, URLVariables, and URLLoader classes. In JavaScript code within an AIR application, you can also use an XMLHttpRequest.

Programming a REST-style web service call in Haxe, typically involves the following steps:

1.  Create a URLRequest object.
2.  Set the service URL and HTTP method verb on the request object.
3.  Create a URLVariables object.
4.  Set the service call parameters as dynamic properties of the variables object.
5.  Assign the variables object to the data property of the request object.
6.  Send the call to the service with a URLLoader object.
7.  Handle the complete event dispatched by the URLLoader that indicates that the service call is complete. It is also wise to listen for the various error events that can be dispatched by a URLLoader object.

For example, consider a web service that exposes a test method that echoes the call parameters back to the requestor. The following Haxe code could be used to call the service:

import openfl.events.Event;
import openfl.events.ErrorEvent;

import openfl.events.IOErrorEvent;
import openfl.events.SecurityErrorEvent;
import openfl.net.URLLoader;

import openfl.net.URLRequest;
import openfl.net.URLRequestMethod;
import openfl.net.URLVariables;

private var requestor:URLLoader = new URLLoader();
public function restServiceCall():void

{

//Create the HTTP request object

var request:URLRequest = new URLRequest( "http://service.example.com/" );
request.method = URLRequestMethod.GET;

//Add the URL variables

var variables:URLVariables = new URLVariables();
variables.method = "test.echo";
variables.api_key = "123456ABC";

variables.message = "Able was I, ere I saw Elba."; 
equest.data = variables;

//Initiate the transaction requestor = new URLLoader();

requestor.addEventListener( Event.COMPLETE, httpRequestComplete );
requestor.addEventListener( IOErrorEvent.IOERROR, httpRequestError );
requestor.addEventListener( SecurityErrorEvent.SECURITY_ERROR, httpRequestError );
requestor.load( request );

}

private function httpRequestComplete( event:Event ):void

{

trace( event.target.data );

}

private function httpRequestError( error:ErrorEvent ):void{
	trace( "An error occured: " + error.message );

}

In JavaScript within an AIR application, you can make the same request using the XMLHttpRequest object:

&lt;html&gt;

&lt;head&gt;&lt;title&gt;RESTful web service request&lt;/title&gt;

&lt;script type="text/javascript"&gt;

function makeRequest()

{

var requestDisplay = document.getElementById( "request" ); var resultDisplay = document.getElementById( "result" );

//Create a conveninece object to hold the call properties var request = {};

request.URL = "http://service.example.com/";
request.method = "test.echo";
request.HTTPmethod = "GET";
request.parameters = {};
request.parameters.api_key = "ABCDEF123";

request.parameters.message = "Able was I ere I saw Elba.";
var requestURL = makeURL( request );

xmlhttp = new XMLHttpRequest();

xmlhttp.open( request.HTTPmethod, requestURL, true);
xmlhttp.onreadystatechange = function() {

if (xmlhttp.readyState == 4) {
	resultDisplay.innerHTML = xmlhttp.responseText;

}

}

xmlhttp.send(null);

requestDisplay.innerHTML = requestURL;

}

//Convert the request object into a properly formatted URL
function makeURL( request )

{

var url = request.URL + "?method=" + escape( request.method );
for( var property in request.parameters )

{

url += "&amp;" + property + "=" + escape( request.parameters[property] );

}

return url;

}

&lt;/script&gt;

&lt;/head&gt;

&lt;body onload="makeRequest()"&gt;

&lt;h1&gt;Request:&lt;/h1&gt;

&lt;div id="request"&gt;&lt;/div&gt;

&lt;h1&gt;Result:&lt;/h1&gt;

&lt;div id="result"&gt;&lt;/div&gt;

&lt;/body&gt;

&lt;/html&gt;

## XML-RPC web service requests {#xml-rpc-web-service-requests}

An XML-RPC web service takes its call parameters as an XML document rather than as a set of URL variables. To conduct a transaction with an XML-RPC web service, create a properly formatted XML message and send it to the web service using the HTTP POST method. In addition, you should set the Content-Type header for the request so that the server treats the request data as XML.

The following example illustrates how to use the same web service call shown in the REST example, but this time as an XML-RPC service:

import openfl.events.Event;
import openfl.events.ErrorEvent;

import openfl.events.IOErrorEvent;
import openfl.events.SecurityErrorEvent;
import openfl.net.URLLoader;

import openfl.net.URLRequest;
import openfl.net.URLRequestMethod;
import openfl.net.URLVariables;

public function xmlRPCRequest():void

{

//Create the XML-RPC document var xmlRPC:XML = &lt;methodCall&gt;

&lt;methodName&gt;&lt;/methodName&gt;

&lt;params&gt;

&lt;param&gt;

&lt;value&gt;

&lt;struct/&gt;

&lt;/value&gt;

&lt;/param&gt;

&lt;/params&gt;

&lt;/methodCall&gt;; xmlRPC.methodName = "test.echo";

//Add the method parameters

var parameters:Object = new Object(); parameters.api_key = "123456ABC"; parameters.message = "Able was I, ere I saw Elba.";

for( var propertyName:String in parameters )

{

xmlRPC..struct.member[xmlRPC..struct.member.length + 1] =

&lt;member&gt;

&lt;name&gt;{propertyName}&lt;/name&gt;

&lt;value&gt;

&lt;string&gt;{parameters[propertyName]}&lt;/string&gt;

&lt;/value&gt;

&lt;/member&gt;;

}

//Create the HTTP request object

var request:URLRequest = new URLRequest( ["http://service.example.com/xml-rpc/"](http://service.example.com/xml-rpc/) ); request.method = URLRequestMethod.POST;

request.cacheResponse = false;

request.requestHeaders.push(new URLRequestHeader("Content-Type", "application/xml"));

request.data = xmlRPC;

//Initiate the request requestor = new URLLoader();

requestor.dataFormat = URLLoaderDataFormat.TEXT; requestor.addEventListener( Event.COMPLETE, xmlRPCRequestComplete ); requestor.addEventListener( IOErrorEvent.IO_ERROR, xmlRPCRequestError );

requestor.addEventListener( SecurityErrorEvent.SECURITY_ERROR, xmlRPCRequestError ); requestor.load( request );

}

private function xmlRPCRequestComplete( event:Event ):void

{

trace( XML(event.target.data).toXMLString() );

}

private function xmlRPCRequestError( error:ErrorEvent ):void

{

trace( "An error occurred: " + error );

}

WebKit in AIR doesn’t support E4X syntax, so the method used to create the XML document in the previous example does not work in JavaScript code. Instead, you must use the DOM methods to create the XML document or create the document as a string and use the JavaScript DOMParser class to convert the string to XML.

The following example uses DOM methods to create an XML-RPC message and an XMLHttpRequest to conduct the web service transaction:

&lt;html&gt;

&lt;head&gt;

&lt;title&gt;XML-RPC web service request&lt;/title&gt;

&lt;script type="text/javascript"&gt;

function makeRequest()

{

var requestDisplay = document.getElementById( "request" );
var resultDisplay = document.getElementById( "result" );

var request = {};

request.URL = "http://services.example.com/xmlrpc/";
request.method = "test.echo";

request.HTTPmethod = "POST";
request.parameters = {};
request.parameters.api_key = "123456ABC";

request.parameters.message = "Able was I ere I saw Elba.";
var requestMessage = formatXMLRPC( request );

xmlhttp = new XMLHttpRequest();

xmlhttp.open( request.HTTPmethod, request.URL, true);
xmlhttp.onreadystatechange = function() {

if (xmlhttp.readyState == 4) {
	resultDisplay.innerText = xmlhttp.responseText;

}

}

xmlhttp.send( requestMessage );

requestDisplay.innerText = xmlToString( requestMessage.documentElement );

}

//Formats a request as XML-RPC document function formatXMLRPC( request )

{

var xmldoc = document.implementation.createDocument( "", "", null );
var root = xmldoc.createElement( "methodCall" );

xmldoc.appendChild( root );

var methodName = xmldoc.createElement( "methodName" );

var methodString = xmldoc.createTextNode( request.method );
methodName.appendChild( methodString );

root.appendChild( methodName );

var params = xmldoc.createElement( "params" );
root.appendChild( params );

var param = xmldoc.createElement( "param" );
params.appendChild( param );

var value = xmldoc.createElement( "value" );
param.appendChild( value );

var struct = xmldoc.createElement( "struct" );
value.appendChild( struct );

for( var property in request.parameters )

{

var member = xmldoc.createElement( "member" );
struct.appendChild( member );

var name = xmldoc.createElement( "name" );

var paramName = xmldoc.createTextNode( property );
name.appendChild( paramName )

member.appendChild( name );

var value = xmldoc.createElement( "value" );
var type = xmldoc.createElement( "string" );
value.appendChild( type );

var paramValue = xmldoc.createTextNode( request.parameters[property] );
type.appendChild( paramValue )

member.appendChild( value );

}

return xmldoc;

}

//Returns a string representation of an XML node function xmlToString( rootNode, indent )

{

if( indent == null ) indent = "";

var result = indent + "&lt;" + rootNode.tagName + "&gt;\n";
for( var i = 0; i &lt; rootNode.childNodes.length; i++)

{

if(rootNode.childNodes.item( i ).nodeType == Node.TEXT_NODE )

{

result += indent + " " + rootNode.childNodes.item( i ).textContent + "\n";

}

}

if( rootNode.childElementCount &gt; 0 )

{

result += xmlToString( rootNode.firstElementChild, indent + " " );

}

if( rootNode.nextElementSibling )

{

result += indent + "&lt;/" + rootNode.tagName + "&gt;\n";

result += xmlToString( rootNode.nextElementSibling, indent );

}

else

{

result += indent +"&lt;/" + rootNode.tagName + "&gt;\n";

}

return result;

}

&lt;/script&gt;

&lt;/head&gt;

&lt;body onload="makeRequest()"&gt;

&lt;h1&gt;Request:&lt;/h1&gt;

&lt;pre id="request"&gt;&lt;/pre&gt;

&lt;h1&gt;Result:&lt;/h1&gt;

&lt;pre id="result"&gt;&lt;/pre&gt;

&lt;/body&gt;

&lt;/html&gt;

## SOAP web service requests {#soap-web-service-requests}

SOAP builds on the general XML-RPC web service concept and provides a richer, albeit more complex, means for transferring typed data. SOAP web services typically provide a Web Service Description Language file (WSDL) that specifies the web service calls, data types, and service URL. While Haxe 3 does not provide direct support for SOAP, you can construct a SOAP XML message "by hand," post it to the server, and then parse the results. However, for anything except the simplest SOAP web service, you can probably save a significant amount of development time using an existing SOAP library.

The Flex framework includes libraries for accessing SOAP web services. In Flash Builder the library, rpc.swc, is automatically included in Flex projects, since it is part of the Flex framework. In Flash Professional, you can add the Flex framework.swc and rpc.swc to the library path of a project and then access the Flex classes with Haxe.

## More Help topics

[Using the Flex web service component in Flash Professional](http://tv.adobe.com/watch/adc-presents/use-the-flex-webservice-component-in-flash/) [Cristophe Coenraets: Real-time Trader Desktop for Android](http://coenraets.org/blog/air-for-android-samples/real-time-trader-desktop-for-android/)