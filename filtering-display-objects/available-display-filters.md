# Available display filters {#available-display-filters}

Haxe includes ten filter classes that you can apply to display objects and BitmapData objects:

*   Bevel filter (BevelFilter class)
*   Blur filter (BlurFilter class)
*   Drop shadow filter (DropShadowFilter class)
*   Glow filter (GlowFilter class)
*   Gradient bevel filter (GradientBevelFilter class)
*   Gradient glow filter (GradientGlowFilter class)
*   Color matrix filter (ColorMatrixFilter class)
*   Convolution filter (ConvolutionFilter class)
*   Displacement map filter (DisplacementMapFilter class)
*   Shader filter (ShaderFilter class)

The first six filters are simple filters that can be used to create one specific effect, with some customization of the effect available. Those six filters can be applied using Haxe, and can also be applied to objects in Flash Professional using the Filters panel. Consequently, even if you’re applying filters using Haxe, if you have Flash Professional you can use the visual interface to quickly try out different filters and settings to figure out how to create a desired effect.

The final four filters are available in Haxe only. Those filters, the color matrix filter, convolution filter, displacement map filter, and shader filter, are much more flexible in the types of effects that they can be used to create. Rather than being optimized for a single effect, they provide power and flexibility. For example, by selecting different values for its matrix, the convolution filter can be used to create effects such as blurring, embossing, sharpening, finding color edges, transformations, and more.

Each of the filters, whether simple or complex, can be customized using their properties. Generally, you have two choices for setting filter properties. All the filters let you set the properties by passing parameter values to the filter object’s constructor. Alternatively, whether or not you set the filter properties by passing parameters, you can adjust the filters later by setting values for the filter object’s properties. Most of the example code listings set the properties directlyto make the example easier to follow. Nevertheless, you could usually achieve the same result in fewer lines of code by passing the values as parameters in the filter object’s constructor. For more details on the specifics of each filter, its properties and its constructor parameters, see the listings for the flash.filters package in the [Haxe](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/filters/package-detail.html) [Reference for the Adobe Flash Platform](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/filters/package-detail.html).

**Bevel filter**

The BevelFilter class allows you to add a 3D beveled edge to the filtered object. This filter makes the hard corners or edges of your object look like they have been chiseled, or beveled, away.

The BevelFilter class properties allow you to customize the appearance of the bevel. You can set highlight and shadow colors, bevel edge blurs, bevel angles, and bevel edge placement; you can even create a knockout effect.

The following example loads an external image and applies a bevel filter to it.

import flash.display.*;

import flash.filters.BevelFilter;

import flash.filters.BitmapFilterQuality; import flash.filters.BitmapFilterType; import flash.net.URLRequest;

// Load an image onto the Stage.

var imageLoader:Loader = new Loader();

var url:String = [&quot;http://www.helpexamples.com/flash/images/image3.jpg&quot;;](http://www.helpexamples.com/flash/images/image3.jpg) var urlReq:URLRequest = new URLRequest(url);

imageLoader.load(urlReq); addChild(imageLoader);

// Create the bevel filter and set filter properties. var bevel:BevelFilter = new BevelFilter();

bevel.distance = 5;

bevel.angle = 45; bevel.highlightColor = 0xFFFF00; bevel.highlightAlpha = 0.8; bevel.shadowColor = 0x666666; bevel.shadowAlpha = 0.8;

bevel.blurX = 5;

bevel.blurY = 5;

bevel.strength = 5;

bevel.quality = BitmapFilterQuality.HIGH; bevel.type = BitmapFilterType.INNER; bevel.knockout = false;

// Apply filter to the image. imageLoader.filters = [bevel];

## Blur filter {#blur-filter}

The BlurFilter class smears, or blurs, a display object and its contents. Blur effects are useful for giving the impression that an object is out of focus or for simulating fast movement, as in a motion blur. By setting the quality property of the blur filter too low, you can simulate a softly out-of-focus lens effect. Setting the quality property to high results in a smooth blur effect similar to a Gaussian blur.

The following example creates a circle object using the drawCircle() method of the Graphics class and applies a blur filter to it:

import flash.display.Sprite;

import flash.filters.BitmapFilterQuality; import flash.filters.BlurFilter;

// Draw a circle.

var redDotCutout:Sprite = new Sprite(); redDotCutout.graphics.lineStyle(); redDotCutout.graphics.beginFill(0xFF0000); redDotCutout.graphics.drawCircle(145, 90, 25); redDotCutout.graphics.endFill();

// Add the circle to the display list. addChild(redDotCutout);

// Apply the blur filter to the rectangle. var blur:BlurFilter = new BlurFilter(); blur.blurX = 10;

blur.blurY = 10;

blur.quality = BitmapFilterQuality.MEDIUM; redDotCutout.filters = [blur];

## Drop shadow filter {#drop-shadow-filter}

Drop shadows give the impression that there is a separate light source situated above a target object. The position and intensity of this light source can be modified to produce a variety of different drop shadow effects.

The DropShadowFilter class uses an algorithm that is similar to the blur filter’s algorithm. The main difference is that the drop shadow filter has a few more properties that you can modify to simulate different light-source attributes (such as alpha, color, offset and brightness).

The drop shadow filter also allows you to apply custom transformation options on the style of the drop shadow, including inner or outer shadow and knockout (also known as cutout) mode.

The following code creates a square box sprite and applies a drop shadow filter to it:

import flash.display.Sprite;

import flash.filters.DropShadowFilter;

// Draw a box.

var boxShadow:Sprite = new Sprite(); boxShadow.graphics.lineStyle(1); boxShadow.graphics.beginFill(0xFF3300); boxShadow.graphics.drawRect(0, 0, 100, 100); boxShadow.graphics.endFill(); addChild(boxShadow);

// Apply the drop shadow filter to the box.

var shadow:DropShadowFilter = new DropShadowFilter(); shadow.distance = 10;

shadow.angle = 25;

// You can also set other properties, such as the shadow color,

// alpha, amount of blur, strength, quality, and options for

// inner shadows and knockout effects. boxShadow.filters = [shadow];

## Glow filter {#glow-filter}

The GlowFilter class applies a lighting effect to display objects, making it appear that a light is being shined up from underneath the object to create a soft glow.

Similar to the drop shadow filter, the glow filter includes properties to modify the distance, angle, and color of the light source to produce varying effects. The GlowFilter also has several options for modifying the style of the glow, including inner or outer glow and knockout mode.

The following code creates a cross using the Sprite class and applies a glow filter to it:

import flash.display.Sprite;

import flash.filters.BitmapFilterQuality; import flash.filters.GlowFilter;

// Create a cross graphic.

var crossGraphic:Sprite = new Sprite(); crossGraphic.graphics.lineStyle(); crossGraphic.graphics.beginFill(0xCCCC00); crossGraphic.graphics.drawRect(60, 90, 100, 20);

crossGraphic.graphics.drawRect(100, 50, 20, 100); crossGraphic.graphics.endFill(); addChild(crossGraphic);

// Apply the glow filter to the cross shape. var glow:GlowFilter = new GlowFilter(); glow.color = 0x009922;

glow.alpha = 1;

glow.blurX = 25;

glow.blurY = 25;

glow.quality = BitmapFilterQuality.MEDIUM; crossGraphic.filters = [glow];

## Gradient bevel filter {#gradient-bevel-filter}

The GradientBevelFilter class lets you apply an enhanced bevel effect to display objects or BitmapData objects. Using a gradient color on the bevel greatly improves the spatial depth of the bevel, giving edges a more realistic, 3D appearance.

The following code creates a rectangle object using the drawRect() method of the Shape class and applies a gradient bevel filter to it.

import flash.display.Shape;

import flash.filters.BitmapFilterQuality; import flash.filters.GradientBevelFilter;

// Draw a rectangle.

var box:Shape = new Shape(); box.graphics.lineStyle(); box.graphics.beginFill(0xFEFE78); box.graphics.drawRect(100, 50, 90, 200); box.graphics.endFill();

// Apply a gradient bevel to the rectangle.

var gradientBevel:GradientBevelFilter = new GradientBevelFilter();

gradientBevel.distance = 8;

gradientBevel.angle = 225; // opposite of 45 degrees gradientBevel.colors = [0xFFFFCC, 0xFEFE78, 0x8F8E01]; gradientBevel.alphas = [1, 0, 1];

gradientBevel.ratios = [0, 128, 255];

gradientBevel.blurX = 8;

gradientBevel.blurY = 8;

gradientBevel.quality = BitmapFilterQuality.HIGH;

// Other properties let you set the filter strength and set options

// for inner bevel and knockout effects. box.filters = [gradientBevel];

// Add the graphic to the display list. addChild(box);

## Gradient glow filter {#gradient-glow-filter}

The GradientGlowFilter class lets you apply an enhanced glow effect to display objects or BitmapData objects. The effect gives you greater color control of the glow, and in turn produces a more realistic glow effect. Additionally, the gradient glow filter allows you to apply a gradient glow to the inner, outer, or upper edges of an object.

The following example draws a circle on the Stage, and applies a gradient glow filter to it. As you move the mouse further to the right and down, the amount of blur increases in the horizontal and vertical directions respectively. In addition, any time you click on the Stage, the strength of the blur increases.

import flash.events.MouseEvent;

import flash.filters.BitmapFilterQuality; import flash.filters.BitmapFilterType; import flash.filters.GradientGlowFilter;

// Create a new Shape instance. var shape:Shape = new Shape();

// Draw the shape. shape.graphics.beginFill(0xFF0000, 100);

shape.graphics.moveTo(0, 0);

shape.graphics.lineTo(100, 0);

shape.graphics.lineTo(100, 100);

shape.graphics.lineTo(0, 100);

shape.graphics.lineTo(0, 0); shape.graphics.endFill();

// Position the shape on the Stage. addChild(shape);

shape.x = 100;

shape.y = 100;

// Define a gradient glow.

var gradientGlow:GradientGlowFilter = new GradientGlowFilter(); gradientGlow.distance = 0;

gradientGlow.angle = 45; gradientGlow.colors = [0x000000, 0xFF0000]; gradientGlow.alphas = [0, 1];

gradientGlow.ratios = [0, 255];

gradientGlow.blurX = 10;

gradientGlow.blurY = 10;

gradientGlow.strength = 2;

gradientGlow.quality = BitmapFilterQuality.HIGH; gradientGlow.type = BitmapFilterType.OUTER;

// Define functions to listen for two events. function onClick(event:MouseEvent):void

{

gradientGlow.strength++; shape.filters = [gradientGlow];

}

function onMouseMove(event:MouseEvent):void

{

gradientGlow.blurX = (stage.mouseX / stage.stageWidth) * 255; gradientGlow.blurY = (stage.mouseY / stage.stageHeight) * 255; shape.filters = [gradientGlow];

}

stage.addEventListener(MouseEvent.CLICK, onClick); stage.addEventListener(MouseEvent.MOUSE_MOVE, onMouseMove);

## Example: Combining basic filters {#example-combining-basic-filters}

The following code example uses several basic filters, combined with a Timer for creating repeating actions, to create an animated traffic light simulation.

import flash.display.Shape; import flash.events.TimerEvent;

import flash.filters.BitmapFilterQuality; import flash.filters.BitmapFilterType; import flash.filters.DropShadowFilter; import flash.filters.GlowFilter;

import flash.filters.GradientBevelFilter; import flash.utils.Timer;

var count:Number = 1; var distance:Number = 8;

var angleInDegrees:Number = 225; // opposite of 45 degrees var colors:Array = [0xFFFFCC, 0xFEFE78, 0x8F8E01];

var alphas:Array = [1, 0, 1];

var ratios:Array = [0, 128, 255]; var blurX:Number = 8;

var blurY:Number = 8; var strength:Number = 1;

var quality:Number = BitmapFilterQuality.HIGH; var type:String = BitmapFilterType.INNER;

var knockout:Boolean = false;

// Draw the rectangle background for the traffic light. var box:Shape = new Shape();

box.graphics.lineStyle(); box.graphics.beginFill(0xFEFE78); box.graphics.drawRect(100, 50, 90, 200); box.graphics.endFill();

// Draw the 3 circles for the three lights. var stopLight:Shape = new Shape(); stopLight.graphics.lineStyle(); stopLight.graphics.beginFill(0xFF0000); stopLight.graphics.drawCircle(145,90,25); stopLight.graphics.endFill();

var cautionLight:Shape = new Shape(); cautionLight.graphics.lineStyle(); cautionLight.graphics.beginFill(0xFF9900); cautionLight.graphics.drawCircle(145,150,25); cautionLight.graphics.endFill();

var goLight:Shape = new Shape(); goLight.graphics.lineStyle(); goLight.graphics.beginFill(0x00CC00); goLight.graphics.drawCircle(145,210,25); goLight.graphics.endFill();

// Add the graphics to the display list. addChild(box);

addChild(stopLight); addChild(cautionLight); addChild(goLight);

// Apply a gradient bevel to the traffic light rectangle.

var gradientBevel:GradientBevelFilter = new GradientBevelFilter(distance, angleInDegrees, colors, alphas, ratios, blurX, blurY, strength, quality, type, knockout);

box.filters = [gradientBevel];

// Create the inner shadow (for lights when off) and glow

// (for lights when on).

var innerShadow:DropShadowFilter = new DropShadowFilter(5, 45, 0, 0.5, 3, 3, 1, 1, true, false);

var redGlow:GlowFilter = new GlowFilter(0xFF0000, 1, 30, 30, 1, 1, false, false);

var yellowGlow:GlowFilter = new GlowFilter(0xFF9900, 1, 30, 30, 1, 1, false, false);

var greenGlow:GlowFilter = new GlowFilter(0x00CC00, 1, 30, 30, 1, 1, false, false);

// Set the starting state of the lights (green on, red/yellow off). stopLight.filters = [innerShadow];

cautionLight.filters = [innerShadow]; goLight.filters = [greenGlow];

// Swap the filters based on the count value. function trafficControl(event:TimerEvent):void

{

if (count == 4)

{

count = 1;

}

switch (count)

{

case 1:

stopLight.filters = [innerShadow]; cautionLight.filters = [yellowGlow]; goLight.filters = [innerShadow]; break;

case 2:

stopLight.filters = [redGlow]; cautionLight.filters = [innerShadow]; goLight.filters = [innerShadow]; break;

case 3:

stopLight.filters = [innerShadow]; cautionLight.filters = [innerShadow]; goLight.filters = [greenGlow];

break;

}

count++;

}

// Create a timer to swap the filters at a 3 second interval. var timer:Timer = new Timer(3000, 9); timer.addEventListener(TimerEvent.TIMER, trafficControl); timer.start();

## Color matrix filter {#color-matrix-filter}

The ColorMatrixFilter class is used to manipulate the color and alpha values of the filtered object. This allows you to create saturation changes, hue rotation (shifting a palette from one range of colors to another), luminance-to-alpha changes, and other color manipulation effects using values from one color channel and potentially applying them to other channels.

Conceptually, the filter goes through the pixels in the source image one by one and separates each pixel into its red, green, blue, and alpha components. It then multiplies values provided in the color matrix by each of these values, adding the results together to determine the resulting color value that will be displayed on the screen for that pixel. The matrix property of the filter is an array of 20 numbers that are used in calculating the final color. For details of the specific algorithm used to calculate the color values, see the entry describing the ColorMatrixFilter class’s matrix property in the [Haxe Reference for the Adobe Flash Platform](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/filters/ColorMatrixFilter.html).

## Convolution filter {#convolution-filter}

The ConvolutionFilter class can be used to apply a wide range of imaging transformations to BitmapData objects or display objects, such as blurring, edge detection, sharpening, embossing, and beveling.

The convolution filter conceptually goes through each pixel in the source image one by one and determines the final color of that pixel using the value of the pixel and its surrounding pixels. A matrix, specified as an array of numeric values, indicates to what degree the value of each particular neighboring pixel affects the final resulting value.

Consider the most commonly used type of matrix, which is a three by three matrix. The matrix includes nine values:

| N | N | N |
| --- | --- | --- |
| N | P | N |
| N | N | N |

When the convolution filter is applied to a certain pixel, it will look at the color value of the pixel itself (“P” in the example), as well as the values of the surrounding pixels (labeled “N” in the example). However, by setting values in the matrix, you specify how much priority certain pixels have in affecting the resulting image.

For example, the following matrix, applied using a convolution filter, will leave an image exactly as it was:

| 0 | 0 | 0 |
| --- | --- | --- |
| 0 | 1 | 0 |
| 0 | 0 | 0 |

The reason the image is unchanged is because the original pixel’s value has a relative strength of 1 in determining the final pixel color, while the surrounding pixels’ values have relative strength of 0—meaning their colors don’t affect the final image.

Similarly, this matrix will cause the pixels of an image to shift one pixel to the left:

| 0 | 0 | 0 |
| --- | --- | --- |
| 0 | 0 | 1 |
| 0 | 0 | 0 |

Notice that in this case, the pixel itself has no effect on the final value of the pixel displayed in that location on the final image—only the value of the pixel to the right is used to determine the pixel’s resulting value.

In Haxe, you create the matrix as a combination of an Array instance containing the values and two properties specifying the number of rows and columns in the matrix. The following example loads an image and, when the image finishes loading, applies a convolution filter to the image using the matrix in the previous listing:

// Load an image onto the Stage. var loader:Loader = new Loader();

var url:URLRequest = new [URLRequest(&quot;http://www.helpexamples.com/flash/images/image1.jpg&quot;);](http://www.helpexamples.com/flash/images/image1.jpg) loader.load(url);

this.addChild(loader);

function applyFilter(event:MouseEvent):void

{

// Create the convolution matrix. var matrix:Array = [0, 0, 0,

0, 0, 1,

0, 0, 0];

var convolution:ConvolutionFilter = new ConvolutionFilter(); convolution.matrixX = 3;

convolution.matrixY = 3; convolution.matrix = matrix; convolution.divisor = 1;

loader.filters = [convolution];

}

loader.addEventListener(MouseEvent.CLICK, applyFilter);

Something that isn’t obvious in this code is the effect of using values other than 1 or 0 in the matrix. For example, the same matrix, with the number 8 instead of 1 in the right-hand position, performs the same action (shifting the pixels to the left). In addition, it affects the colors of the image, making them 8 times brighter. This is because the final pixel color values are calculated by multiplying the matrix values by the original pixel colors, adding the values together, and dividing by the value of the filter’s divisor property. Notice that in the example code, the divisor property is set to

1\. As a general rule, if you want the brightness of the colors to stay about the same as in the original image, you should make the divisor equal to the sum of the matrix values. So with a matrix where the values add up to 8, and a divisor of 1, the resulting image is going to be roughly 8 times brighter than the original image.

Although the effect of this matrix isn’t very noticeable, other matrix values can be used to create various effects. Here are several standard sets of matrix values for different effects using a three by three matrix:

*   Basic blur (divisor 5):

0 1 0

1 1 1

0 1 0

*   Sharpening (divisor 1):

0, -1, 0

-1, 5, -1

0, -1, 0

*   Edge detection (divisor 1):

0, -1, 0

-1, 4, -1

0, -1, 0

*   Embossing effect (divisor 1):

-2, -1, 0

-1, 1, 1

0, 1, 2

Notice that with most of these effects, the divisor is 1\. This is because the negative matrix values added to the positive matrix values result in 1 (or 0 in the case of edge detection, but the divisor property’s value cannot be 0).

## Displacement map filter {#displacement-map-filter}

The DisplacementMapFilter class uses pixel values from a BitmapData object (known as the displacement map image) to perform a displacement effect on a new object. The displacement map image is typically different than the actual display object or BitmapData instance to which the filter is being applied. A displacement effect involves displacing pixels in the filtered image—in other words, shifting them away from their original location to some extent. This filter can be used to create a shifted, warped, or mottled effect.

The location and amount of displacement applied to a given pixel is determined by the color value of the displacement map image. When working with the filter, in addition to specifying the map image, you specify the following values to control how the displacement is calculated from the map image:

*   Map point: The location on the filtered image at which the upper-left corner of the displacement filter will be applied. You can use this if you only want to apply the filter to part of an image.
*   X component: Which color channel of the map image affects the x position of pixels.
*   Y component: Which color channel of the map image affects the y position of pixels.
*   X scale: A multiplier value that specifies how strong the x axis displacement is.
*   Y scale: A multiplier value that specifies how strong the y axis displacement is.
*   Filter mode: Determines what to do in any empty spaces created by pixels being shifted away. The options, defined as constants in the DisplacementMapFilterMode class, are to display the original pixels (filter mode IGNORE), to wrap the pixels around from the other side of the image (filter mode WRAP, which is the default), to use the nearest shifted pixel (filter mode CLAMP), or to fill in the spaces with a color (filter mode COLOR).

To understand how the displacement map filter works, consider a basic example. In the following code, an image is loaded, and when it finishes loading it is centered on the Stage and a displacement map filter is applied to it, causing the pixels in the entire image to shift horizontally to the left.

import flash.display.BitmapData; import flash.display.Loader; import flash.events.MouseEvent;

import flash.filters.DisplacementMapFilter; import flash.geom.Point;

import flash.net.URLRequest;

// Load an image onto the Stage. var loader:Loader = new Loader();

var url:URLRequest = new [URLRequest(&quot;http://www.helpexamples.com/flash/images/image3.jpg&quot;);](http://www.helpexamples.com/flash/images/image3.jpg) loader.load(url);

this.addChild(loader);

var mapImage:BitmapData;

var displacementMap:DisplacementMapFilter;

// This function is called when the image finishes loading. function setupStage(event:Event):void

{

// Center the loaded image on the Stage.

loader.x = (stage.stageWidth - loader.width) / 2; loader.y = (stage.stageHeight - loader.height) / 2;

// Create the displacement map image.

mapImage = new BitmapData(loader.width, loader.height, false, 0xFF0000);

// Create the displacement filter. displacementMap = new DisplacementMapFilter(); displacementMap.mapBitmap = mapImage; displacementMap.mapPoint = new Point(0, 0);

displacementMap.componentX = BitmapDataChannel.RED; displacementMap.scaleX = 250;

loader.filters = [displacementMap];

}

loader.contentLoaderInfo.addEventListener(Event.COMPLETE, setupStage);

The properties used to define the displacement are as follows:

*   Map bitmap: The displacement bitmap is a new BitmapData instance created by the code. Its dimensions match the dimensions of the loaded image (so the displacement is applied to the entire image). It is filled with solid red pixels.
*   Map point: This value is set to the point 0, 0—again, causing the displacement to be applied to the entire image.
*   X component: This value is set to the constant BitmapDataChannel.RED, meaning the red value of the map bitmap will determine how much the pixels are displaced (how much they move) along the x axis.
*   X scale: This value is set to 250\. The full amount of displacement (from the map image being completely red) only displaces the image by a small amount (roughly one-half of a pixel), so if this value was set to 1 the image would only shift .5 pixels horizontally. By setting it to 250, the image shifts by approximately 125 pixels.

These settings cause the filtered image’s pixels to shift 250 pixels to the left. The direction (left or right) and amount of shift is based on the color value of the pixels in the map image. Conceptually, the filter goes through the pixels of the filtered image one by one (at least, the pixels in the region where the filter is applied, which in this case means all the pixels), and does the following with each pixel:

1.  It finds the corresponding pixel in the map image. For example, when the filter calculates the displacement amount for the pixel in the upper-left corner of the filtered image, it looks at the pixel in the upper-left corner of the map image.
2.  It determines the value of the specified color channel in the map pixel. In this case, the x component color channel is the red channel, so the filter looks to see what the value of the red channel of the map image is at the pixel in question. Since the map image is solid red, the pixel’s red channel is 0xFF, or 255\. This is used as the displacement value.
3.  It compares the displacement value to the “middle” value (127, which is halfway between 0 and 255). If the displacement value is lower than the middle value, the pixel shifts in a positive direction (to the right for x displacement; down for y displacement). On the other hand, if the displacement value is higher than the middle value (as in this example), the pixel shifts in a negative direction (to the left for x displacement; up for y displacement). To be more precise, the filter subtracts the displacement value from 127, and the result (positive or negative) is the relative amount of displacement that is applied.
4.  Finally, it determines the actual amount of displacement by determining what percentage of full displacement the relative displacement value represents. In this case, full red means 100% displacement. That percentage is then multiplied by the x scale or y scale value to determine the number of pixels of displacement that will be applied. In this example, 100% times a multiplier of 250 determines the amount of displacement—roughly 125 pixels to the left.

Because no values are specified for y component and y scale, the defaults (which cause no displacement) are used— that’s why the image doesn’t shift at all in the vertical direction.

The default filter mode setting, WRAP, is used in the example, so as the pixels shift to the left the empty space on the right is filled in by the pixels that shifted off the left edge of the image. You can experiment with this value to see the different effects. For example, if you add the following line to the portion of code where the displacement properties are being set (before the line loader.filters = [displacementMap]), it will make the image look as though it has been smeared across the Stage:

displacementMap.mode = DisplacementMapFilterMode.CLAMP;

For a more complex example, the following listing uses a displacement map filter to create a magnifying glass effect on an image:

import flash.display.Bitmap; import flash.display.BitmapData;

import flash.display.BitmapDataChannel; import flash.display.GradientType; import flash.display.Loader;

import flash.display.Shape; import flash.events.MouseEvent;

import flash.filters.DisplacementMapFilter; import flash.filters.DisplacementMapFilterMode; import flash.geom.Matrix;

import flash.geom.Point; import flash.net.URLRequest;

// Create the gradient circles that will together form the

// displacement map image var radius:uint = 50;

var type:String = GradientType.LINEAR;

var redColors:Array = [0xFF0000, 0x000000]; var blueColors:Array = [0x0000FF, 0x000000]; var alphas:Array = [1, 1];

var ratios:Array = [0, 255];

var xMatrix:Matrix = new Matrix(); xMatrix.createGradientBox(radius * 2, radius * 2); var yMatrix:Matrix = new Matrix();

yMatrix.createGradientBox(radius * 2, radius * 2, Math.PI / 2);

var xCircle:Shape = new Shape(); xCircle.graphics.lineStyle(0, 0, 0);

xCircle.graphics.beginGradientFill(type, redColors, alphas, ratios, xMatrix); xCircle.graphics.drawCircle(radius, radius, radius);

var yCircle:Shape = new Shape(); yCircle.graphics.lineStyle(0, 0, 0);

yCircle.graphics.beginGradientFill(type, blueColors, alphas, ratios, yMatrix); yCircle.graphics.drawCircle(radius, radius, radius);

// Position the circles at the bottom of the screen, for reference. this.addChild(xCircle);

xCircle.y = stage.stageHeight - xCircle.height; this.addChild(yCircle);

yCircle.y = stage.stageHeight - yCircle.height; yCircle.x = 200;

// Load an image onto the Stage. var loader:Loader = new Loader();

var url:URLRequest = new [URLRequest(&quot;http://www.helpexamples.com/flash/images/image1.jpg&quot;);](http://www.helpexamples.com/flash/images/image1.jpg) loader.load(url);

this.addChild(loader);

// Create the map image by combining the two gradient circles.

var map:BitmapData = new BitmapData(xCircle.width, xCircle.height, false, 0x7F7F7F); map.draw(xCircle);

var yMap:BitmapData = new BitmapData(yCircle.width, yCircle.height, false, 0x7F7F7F); yMap.draw(yCircle);

map.copyChannel(yMap, yMap.rect, new Point(0, 0), BitmapDataChannel.BLUE, BitmapDataChannel.BLUE);

yMap.dispose();

// Display the map image on the Stage, for reference. var mapBitmap:Bitmap = new Bitmap(map); this.addChild(mapBitmap);

mapBitmap.x = 400;

mapBitmap.y = stage.stageHeight - mapBitmap.height;

// This function creates the displacement map filter at the mouse location. function magnify():void

{

// Position the filter.

var filterX:Number = (loader.mouseX) - (map.width / 2); var filterY:Number = (loader.mouseY) - (map.height / 2); var pt:Point = new Point(filterX, filterY);

var xyFilter:DisplacementMapFilter = new DisplacementMapFilter(); xyFilter.mapBitmap = map;

xyFilter.mapPoint = pt;

// The red in the map image will control x displacement. xyFilter.componentX = BitmapDataChannel.RED;

// The blue in the map image will control y displacement. xyFilter.componentY = BitmapDataChannel.BLUE; xyFilter.scaleX = 35;

xyFilter.scaleY = 35;

xyFilter.mode = DisplacementMapFilterMode.IGNORE; loader.filters = [xyFilter];

}

// This function is called when the mouse moves. If the mouse is

// over the loaded image, it applies the filter. function moveMagnifier(event:MouseEvent):void

{

if (loader.hitTestPoint(loader.mouseX, loader.mouseY))

{

magnify();

}

}

loader.addEventListener(MouseEvent.MOUSE_MOVE, moveMagnifier);

The code first generates two gradient circles, which are combined together to form the displacement map image. The red circle creates the x axis displacement (xyFilter.componentX = BitmapDataChannel.RED), and the blue circle creates the y axis displacement (xyFilter.componentY = BitmapDataChannel.BLUE). To help you understand what the displacement map image looks like, the code adds the original circles as well as the combined circle that serves as the map image to the bottom of the screen.

![Image showing a photo of a flower with a circle-shaped portion under the mouse cursor magnified.](C:\Development\Books\HaxeDevelopersGuide\export\assets\image_showing_a_photo_of_a_flower_w.jpeg)

The code then loads an image and, as the mouse moves, applies the displacement filter to the portion of the image that’s under the mouse. The gradient circles used as the displacement map image causes the displaced region to spread out away from the pointer. Notice that the gray regions of the displacement map image don’t cause any displacement. The gray color is 0x7F7F7F. The blue and red channels of that shade of gray exactly match the middle shade of those color channels, so there is no displacement in a gray area of the map image. Likewise, in the center of the circle there is no displacement. Although the color there isn’t gray, that color’s blue channel and red channel are identical to the blue channel and red channel of medium gray, and since blue and red are the colors that cause displacement, no displacement happens there.

## Shader filter {#shader-filter}

OpenFL 10 and later, Adobe AIR 1.5 and later

The ShaderFilter class lets you use a custom filter effect defined as a Pixel Bender shader. Because the filter effect is written as a Pixel Bender shader, the effect can be completely customized. The filtered content is passed in to the shader as an image input, and the result of the shader operation becomes the filter result.

**_Note:_ **_The Shader filter is available in Haxe starting with OpenFL 10 and Adobe AIR 1.5._

To apply a shader filter to an object, you first create a Shader instance representing the Pixel Bender shader that you are using. For details on the procedure for creating a Shader instance and on how to specify input image and parameter values, see

“Working with Pixel Bender shaders” on page 300

.

When using a shader as a filter, there are three important things to keep in mind:

*   The shader must be defined to accept at least one input image.
*   The filtered object (the display object or BitmapData object to which the filter is applied) is passed to the shader as the first input image value. Because of this, do not manually specify a value for the first image input.
*   If the shader defines more that one input image, the additional inputs must be specified manually (that is, by setting the input property of any ShaderInput instance that belongs to the Shader instance).

Once you have a Shader object for your shader, you create a ShaderFilter instance. This is the actual filter object that you use like any other filter. To create a ShaderFilter that uses a Shader object, call the ShaderFilter() constructor and pass the Shader object as an argument, as shown in this listing:

var myFilter:ShaderFilter = new ShaderFilter(myShader);

For a complete example of using a shader filter, see

“Using a shader as a filter” on page 318

.