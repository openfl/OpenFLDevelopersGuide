# XML-RPC web service requests

An XML-RPC web service takes its call parameters as an XML document rather than
as a set of URL variables. To conduct a transaction with an XML-RPC web service,
create a properly formatted XML message and send it to the web service using the
HTTP `POST` method. In addition, you should set the `Content-Type` header for
the request so that the server treats the request data as XML.

The following example illustrates how to use the same web service call shown in
the REST example, but this time as an XML-RPC service:

```haxe
import openfl.display.Sprite;
import openfl.events.ErrorEvent;
import openfl.events.Event;
import openfl.events.IOErrorEvent;
import openfl.events.SecurityErrorEvent;
import openfl.net.URLLoader;
import openfl.net.URLLoaderDataFormat;
import openfl.net.URLRequest;
import openfl.net.URLRequestHeader;
import openfl.net.URLRequestMethod;

class XmlRpcWebServiceExample extends Sprite {
	private var requestor:URLLoader = new URLLoader();

	public function new() {
		super();

		// Create the XML-RPC document
		var xmlRPC = Xml.parse('<methodCall></methodCall>');
		var xmlMethodCall = xmlRPC.firstElement();

		var xmlMethodName = Xml.createElement("methodName");
		xmlMethodName.addChild(Xml.createPCData("test.echo"));
		xmlMethodCall.addChild(xmlMethodName);

		// Add the method parameters

		var parameters:Map<String, String> = ["api_key" => "123456ABC", "message" => "Able was I, ere I saw Elba."];

		var xmlParams = Xml.createElement("params");
		xmlMethodCall.addChild(xmlParams);
		var xmlParam = Xml.createElement("param");
		xmlParams.addChild(xmlParam);
		var xmlValue = Xml.createElement("value");
		xmlParam.addChild(xmlValue);
		var xmlStruct = Xml.createElement("struct");
		xmlValue.addChild(xmlStruct);
		for (key => value in parameters) {
			var xmlMember = Xml.createElement("member");
			xmlStruct.addChild(xmlMember);

			var xmlName = Xml.createElement("name");
			xmlName.addChild(Xml.createPCData(key));
			xmlMember.addChild(xmlName);

			var xmlValue = Xml.createElement("value");
			xmlValue.addChild(Xml.createPCData(value));
			xmlMember.addChild(xmlValue);
		}

		// Create the HTTP request object
		var request:URLRequest = new URLRequest("http://service.example.com/xml-rpc.php");
		request.method = URLRequestMethod.POST;
		request.requestHeaders.push(new URLRequestHeader("Content-Type", "application/xml"));
		request.data = xmlRPC.toString();

		// Initiate the request
		requestor = new URLLoader();
		requestor.dataFormat = URLLoaderDataFormat.TEXT;
		requestor.addEventListener(Event.COMPLETE, xmlRPCRequestComplete);
		requestor.addEventListener(IOErrorEvent.IO_ERROR, xmlRPCRequestError);
		requestor.addEventListener(SecurityErrorEvent.SECURITY_ERROR, xmlRPCRequestError);
		requestor.load(request);
	}

	private function xmlRPCRequestComplete(event:Event):Void {
		trace(Xml.parse(event.target.data));
	}

	private function xmlRPCRequestError(event:ErrorEvent):Void {
		trace("An error occurred: " + event);
		trace(event.target.data);
	}
}
```


The following code contains the contents of the PHP _xml-rpc.php_ document used
in the previous example:

```php
<?php
	$method = NULL;
	$api_key = NULL;
	$input = file_get_contents('php://input');

	if (empty($input)) {
		http_response_code(400);
		echo "Invalid request";
		exit(1);
	}

	$xml_input = new SimpleXMLElement($input);

	$method = $xml_input->methodName;
	if ($method != NULL && strcmp($method, "test.echo") == 0) {
		$message = NULL;
		foreach ($xml_input->params->param->value->struct->member as $member) {
			$memberName = $member->name;
			$memberValue = $member->value;

			if (strcmp($memberName, "api_key") == 0) {
				if ($memberValue == NULL || strcmp($memberValue, "123456ABC") != 0) {
					http_response_code(400);
					echo "Invalid API key";
					exit(1);
				}
			} else if (strcmp($memberName, "message") == 0) {
				$message = $memberValue;
			} else {
				http_response_code(400);
				echo "Unknown member";
				exit(1);
			}
		}
		echo $message;
	} else {
		http_response_code(400);
		echo "Unknown method";
		exit(1);
	}
?>
```