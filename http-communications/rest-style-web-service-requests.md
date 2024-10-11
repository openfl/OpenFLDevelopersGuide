# REST-style web service requests

REST-style web services use HTTP method verbs to designate the basic action and
URL variables to specify the action details. For example, a request to get data
for an item could use the GET verb and URL variables to specify a method name
and item ID. The resulting URL string might look like:

    http://service.example.com/?method=getItem&id=d3452

To access a REST-style web service with OpenFL, you can use the
URLRequest, URLVariables, and URLLoader classes.

Programming a REST-style web service call in Haxe typically involves
the following steps:

1.  Create a URLRequest object.

2.  Set the service URL and HTTP method verb on the request object.

3.  Create a URLVariables object.

4.  Set the service call parameters as dynamic properties of the variables
    object.

5.  Assign the variables object to the data property of the request object.

6.  Send the call to the service with a URLLoader object.

7.  Handle the `complete` event dispatched by the URLLoader that indicates that
    the service call is complete. It is also wise to listen for the various
    error events that can be dispatched by a URLLoader object.

For example, consider a web service that exposes a test method that echoes the
call parameters back to the requestor. The following Haxe code could be
used to call the service:

```haxe
import openfl.display.Sprite;
import openfl.events.ErrorEvent;
import openfl.events.Event;
import openfl.events.IOErrorEvent;
import openfl.events.SecurityErrorEvent;
import openfl.net.URLLoader;
import openfl.net.URLRequest;
import openfl.net.URLRequestMethod;
import openfl.net.URLVariables;

class RestWebServiceExample extends Sprite {
	private var requestor:URLLoader = new URLLoader();

	public function new() {
		super();

		// Create the HTTP request object
		var request:URLRequest = new URLRequest("http://service.example.com/index.php");
		request.method = URLRequestMethod.GET;

		// Add the URL variables
		var variables:URLVariables = new URLVariables();
		variables.method = "test.echo";
		variables.api_key = "123456ABC";
		variables.message = "Able was I, ere I saw Elba.";
		request.data = variables;

		// Initiate the transaction
		requestor = new URLLoader();
		requestor.addEventListener(Event.COMPLETE, httpRequestComplete);
		requestor.addEventListener(IOErrorEvent.IO_ERROR, httpRequestError);
		requestor.addEventListener(SecurityErrorEvent.SECURITY_ERROR, httpRequestError);
		requestor.load(request);
	}

	private function httpRequestComplete(event:Event):Void {
		trace(event.target.data);
	}

	private function httpRequestError(event:ErrorEvent):Void {
		trace("An error occurred: " + event);
		trace(event.target.data);
	}
}
```

The following code contains the contents of the PHP _index.php_ document used
in the previous example:

```php
<?php
	$method = NULL;
	$api_key = NULL;
	if (!empty($_REQUEST["method"])) {
		$method = $_REQUEST["method"];
	}
	if (!empty($_REQUEST["api_key"])) {
		$api_key = $_REQUEST["api_key"];
	}
	if ($api_key == NULL || strcmp($api_key, "123456ABC") != 0) {
		http_response_code(400);
		echo "Invalid API key";
		exit(1);
	}
	if ($method != NULL && strcmp($method, "test.echo") == 0) {
		if (!empty($_REQUEST["message"])) {
			echo $_REQUEST["message"];
		}
	} else {
		http_response_code(400);
		echo "Unknown method";
		exit(1);
	}
?>
```