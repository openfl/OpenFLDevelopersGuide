# Loading display objects {#loading-display-objects}

Loader objects are used to load projects and graphics files into an application. The Loader class is a subclass of the DisplayObjectContainer class. A Loader object can contain only one child display object in its display list—the display object representing the asset library or graphic file that it loads. When you add a Loader object to the display list, as in the following code, you also add the loaded child display object to the display list once it loads:

```haxe
var pictLdr = new Loader ();
var pictURL = "banana.jpg";
var pictURLReq = new URLRequest (pictURL);
pictLdr.load (pictURLReq);
this.addChild (pictLdr);
```

Once the project or image is loaded, you can move the loaded display object to another display object container, such as the container DisplayObjectContainer object in this example:

```haxe
import openfl.display.*;
import openfl.net.URLRequest;
import openfl.events.Event;

...

var container = new Sprite ();
addChild (container);

var pictLdr = new Loader ();
var pictURL = "banana.jpg";
var pictURLReq = new URLRequest (pictURL);
pictLdr.load (pictURLReq);
pictLdr.contentLoaderInfo.addEventListener (Event.COMPLETE, imgLoaded);

...

private function imgLoaded (event:Event):Void {
	
	container.addChild (pictLdr.content);
	
}
```