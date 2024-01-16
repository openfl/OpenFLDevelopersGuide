# Manipulating pixels {#manipulating-pixels}

The BitmapData class contains a set of methods that allow you to manipulate pixel data values.

## Manipulating individual pixels

When changing the appearance of a bitmap image at a pixel level, you first need to get the color values of the pixels contained within the area you wish to manipulate. You use the `getPixel()` method to read these pixel values.

The `getPixel()` method retrieves an RGB value from a set of x, y (pixel) coordinates that are passed as a parameter. If any of the pixels that you want to manipulate include transparency (alpha channel) information, you need to use the `getPixel32()` method. This method also retrieves an RGB value, but unlike with `getPixel()`, the value returned by `getPixel32()` contains additional data that represents the alpha channel (transparency) value of the selected pixel.

Alternatively, if you simply want to change the color or transparency of a pixel contained within a bitmap, you can use the `setPixel()` or `setPixel32()` method. To set a pixel’s color, simply pass in the x, y coordinates and the color value to one of these methods.

The following example uses `setPixel()` to draw a cross on a green BitmapData background. It then uses `getPixel()` to retrieve the color value from the pixel at the coordinate 50, 50 and traces the returned value.

```haxe
import openfl.display.Bitmap;
import openfl.display.BitmapData;

...

var myBitmapData = new BitmapData (100, 100, false, 0x009900);

for (i in 0...100) {
	
	var red = 0xFF0000;
	myBitmapData.setPixel (50, i, red);
	myBitmapData.setPixel (i, 50, red);
	
}

var myBitmapImage = new Bitmap (myBitmapData);
addChild (myBitmapImage);

var pixelValue = myBitmapData.getPixel (50, 50);
trace (StringTools.hex (pixelValue, 6));
```

If you want to read the value of a group of pixels, as opposed to a single pixel, use the `getPixels()` method. This method generates a byte array from a rectangular region of pixel data that is passed as a parameter. Each of the elements of the byte array (in other words, the pixel values) are unsigned integers—32-bit, unmultiplied pixel values.

Conversely, to change (or set) the value of a group of pixels, use the `setPixels()` method. This method expects two parameters (`rect` and `inputByteArray`), which are combined to output a rectangular region (`rect`) of pixel data (`inputByteArray`).

As data is read (and written) out of the `inputByteArray`, the `ByteArray.readUnsignedInt()` method is called for each of the pixels in the array. If, for some reason, the `inputByteArray` doesn't contain a full rectangle worth of pixel data, the method stops processing the image data at that point.

It's important to remember that, for both getting and setting pixel data, the byte array expects 32-bit alpha, red, green, blue (ARGB) pixel values.

The following example uses the `getPixels()` and `setPixels()` methods to copy a group of pixels from one BitmapData object to another:

```haxe
import openfl.display.Bitmap;
import openfl.display.BitmapData;
import openfl.utils.ByteArray;
import openfl.geom.Rectangle;

...

var bitmapDataObject1 = new BitmapData (100, 100, false, 0x006666FF);
var bitmapDataObject2 = new BitmapData (100, 100, false, 0x00FF0000);

var rect = new Rectangle (0, 0, 100, 100);
var bytes = bitmapDataObject1.getPixels (rect);

bytes.position = 0;
bitmapDataObject2.setPixels (rect, bytes);

var bitmapImage1 = new Bitmap (bitmapDataObject1);
addChild (bitmapImage1);
var bitmapImage2 = new Bitmap (bitmapDataObject2);
addChild (bitmapImage2);
bitmapImage2.x = 110;
```

## Pixel-level collision detection {#pixel-level-collision-detection}

The `BitmapData.hitTest()` method performs pixel-level collision detection between the bitmap data and another object or point.

The `BitmapData.hitTest()` method accepts five parameters:

*   `firstPoint` (Point): This parameter refers to the pixel position of the upper-left corner of the first BitmapData upon which the hit test is being performed.
*   `firstAlphaThreshold` (uint): This parameter specifies the highest alpha channel value that is considered opaque for this hit test.
*   `secondObject` (Object): This parameter represents the area of impact. The `secondObject` object can be a Rectangle, Point, Bitmap, or BitmapData object. This object represents the hit area on which the collision detection is being performed.
*   `secondBitmapDataPoint` (Point): This optional parameter is used to define a pixel location in the second BitmapData object. This parameter is used only when the value of `secondObject` is a BitmapData object. The default is `null`.
*   `secondAlphaThreshold` (uint): This optional parameter represents the highest alpha channel value that is considered opaque in the second BitmapData object. The default value is 1. This parameter is only used when the value of `secondObject` is a BitmapData object and both BitmapData objects are transparent.

When performing collision detection on opaque images, keep in mind that OpenFL treats the image as though it were a fully opaque rectangle (or bounding box). Alternatively, when performing pixel-level hit testing on images that are transparent, both of the images are required to be transparent. In addition to this, OpenFL uses the alpha threshold parameters to determine at what point the pixels change from being transparent to opaque.

The following example creates three bitmap images and checks for pixel collision using two different collision points (one returns `false`, the other `true`):

```haxe
import openfl.display.Bitmap;
import openfl.display.BitmapData;
import openfl.geom.Point;

...

var bmd1 = new BitmapData (100, 100, false, 0x000000FF);
var bmd2 = new BitmapData (20, 20, false, 0x00FF3300);

var bm1 = new Bitmap (bmd1);
this.addChild (bm1);

// Create a red square.
var redSquare1 = new Bitmap (bmd2);
this.addChild (redSquare1);
redSquare1.x = 0;

// Create a second red square.
var redSquare2 = new Bitmap (bmd2);
this.addChild (redSquare2);
redSquare2.x = 150;
redSquare2.y = 150;

// Define the point at the top-left corner of the bitmap.
var pt1 = new Point (0, 0);

// Define the point at the center of redSquare1.
var pt2 = new Point (20, 20);

// Define the point at the center of redSquare2.
var pt3 = new Point (160, 160);

trace (bmd1.hitTest (pt1, 0xFF, pt2)); // true
trace (bmd1.hitTest (pt1, 0xFF, pt3)); // false
```