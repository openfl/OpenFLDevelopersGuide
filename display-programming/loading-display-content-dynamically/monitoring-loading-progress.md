# Monitoring loading progress {#monitoring-loading-progress}

Once the file has started loading, a LoaderInfo object is created. A LoaderInfo object provides information such as load progress, the URLs of the loader and loadee, the number of bytes total for the media, and the nominal height and width of the media. A LoaderInfo object also dispatches events for monitoring the progress of the load.

The following diagram shows the different uses of the LoaderInfo object—for the instance of the main class of the project, for a Loader object, and for an object loaded by the Loader object:

![](/assets/dp_loaderInfo_object_popup.png)

The LoaderInfo object can be accessed as a property of both the Loader object and the loaded display object. As soon as loading begins, the LoaderInfo object can be accessed through the `contentLoaderInfo` property of the Loader object. Once the display object has finished loading, the LoaderInfo object can also be accessed as a property of the loaded display object through the display object’s `loaderInfo` property. The `loaderInfo` property of the loaded display object refers to the same LoaderInfo object as the `contentLoaderInfo` property of the Loader object. In other words, a LoaderInfo object is shared between a loaded object and the Loader object that loaded it (between loader and loadee).

In order to access properties of loaded content, you will want to add an event listener to the LoaderInfo object, as in the following code:

```haxe
import openfl.display.Loader;
import openfl.display.Sprite;
import openfl.events.Event;

...

var ldr = new Loader ();
var urlReq = new URLRequest ("Circle.swf");
ldr.load (urlReq);
ldr.contentLoaderInfo.addEventListener (Event.COMPLETE, loaded);
addChild (ldr);

...

private function loaded (event:Event):Void {
	
	var content:Sprite = event.target.content;
	content.scaleX = 2;
	
}
```

For more information, see ["Handling events"](/handling-events/README.md).