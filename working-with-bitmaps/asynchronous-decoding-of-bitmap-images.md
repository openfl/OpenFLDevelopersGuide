# Asynchronous decoding of bitmap images {#asynchronous-decoding-of-bitmap-images}

OpenFL 11 and later, Adobe AIR 2.6 and later

When you work with bitmap images, you can asynchronously decode and load the bitmap images to improve your application’s perceived performance. Decoding a bitmap image asynchronously can take the same time as decoding the image synchronously in many cases. However, the bitmap image gets decoded in a separate thread before the associated Loader object sends the COMPLETE event. Hence, you can asynchronously decode larger images after loading them.

The ImageDecodingPolicy class in the openfl.system package, allows you to specify the bitmap loading scheme. The default loading scheme is synchronous.

| **Bitmap Decoding Policy** | **Bitmap Loading Scheme** | **Description** |
| --- | --- | --- |
| ImageDecodingPolicy.ON_DEMAND | Synchronous | Loaded images are decoded when the image data is accessed. |
| ImageDecodingPolicy.ON_LOAD | Asynchronous | Loaded images are decoded on load, before the COMPLETE event is dispatched. |

**_Note:_** _If the file being loaded is a bitmap image and the decoding policy used is ON_LOAD, the image is decoded asynchronously before the COMPLETE event is dispatched._

The following code shows the usage of the ImageDecodingPolicy class:

var loaderContext:LoaderContext = new LoaderContext();
loaderContext.imageDecodingPolicy = ImageDecodingPolicy.ON_LOAD;
var loader:Loader = new Loader();

loader.load(new URLRequest("http://www.adobe.com/myimage.png"), loaderContext);

You can still use ON_DEMAND decoding with Loader.load() and Loader.loadBytes() methods. However, all the other methods that take a LoaderContext object as an argument, ignore any ImageDecodingPolicy value passed.

The following example shows the difference in decoding a bitmap image synchronously and asynchronously:

package

{

import openfl.display.Loader;
import openfl.display.Sprite;
import openfl.events.Event;
import openfl.net.URLRequest;

import openfl.system.ImageDecodingPolicy;
import openfl.system.LoaderContext;

public class AsyncTest extends Sprite

{

private var loaderContext:LoaderContext;
private var loader:Loader;

private var urlRequest:URLRequest;
public function AsyncTest()

{

//Load the image synchronously loaderContext = new LoaderContext();

//Default behavior.

loaderContext.imageDecodingPolicy = ImageDecodingPolicy.ON_DEMAND; loader = new Loader();

loadImageSync();

//Load the image asynchronously
loaderContext = new LoaderContext();

loaderContext.imageDecodingPolicy = ImageDecodingPolicy.ON_LOAD;
loader = new Loader();

loadImageASync();

}

private function loadImageASync():void{
	trace("Loading image asynchronously...");

urlRequest = new URLRequest("http://www.adobe.com/myimage.png");
urlRequest.useCache = false;

loader.load(urlRequest, loaderContext);

loader.contentLoaderInfo.addEventListener(Event.COMPLETE, onAsyncLoadComplete);

}

private function onAsyncLoadComplete(event:Event):void{
	trace("Async. Image Load Complete");

}

private function loadImageSync():void{
trace("Loading image synchronously...");

urlRequest = new URLRequest("http://www.adobe.com/myimage.png");
urlRequest.useCache = false;

loader.load(urlRequest, loaderContext);
loader.contentLoaderInfo.addEventListener(Event.COMPLETE, onSyncLoadComplete);

}

private function onSyncLoadComplete(event:Event):void{ trace("Sync. Image Load Complete");

}

}

}

For a demonstration of the effect of the different decoding policies, see [Thibaud Imbert: Asynchronous bitmap](http://www.bytearray.org/?p=2931) [decoding in the Adobe Flash runtimes](http://www.bytearray.org/?p=2931)