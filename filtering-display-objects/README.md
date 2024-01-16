# Chapter 14: Filtering display objects {#chapter-14-filtering-display-objects}

Historically, the application of filter effects to bitmap images has been the domain of specialized image-editing software such as Adobe Photoshop® and Adobe Fireworks®. Haxe includes the openfl.filters package, which contains a series of bitmap effect filter classes. These effects allow developers to programmatically apply filters to bitmaps and display objects and achieve many of the same effects that are available in graphics manipulation applications.

**Basics of filtering display objects**

One of the ways to add polish to an application is to add simple graphic effects. You can add a drop shadow behind a photo to create the illusion of 3-d, or a glow around a button to show that it is active. Haxe includes ten filters that you can apply to any display object or to a BitmapData instance. The built-in filters range from basic, such as the drop shadow and glow filters, to complex, such as the displacement map filter and the convolution filter.

**_Note:_** _In addition to the built-in filters, you can also program custom filters and effects using Pixel Bender. See_

_"Working_

_with Pixel Bender shaders" on page 300_

_._

Important concepts and terms

The following reference list contains important terms that you might encounter when creating filters:

**Bevel** An edge created by lightening pixels on two sides and darkening pixels on the opposite two sides. This effect creates the appearance of a three-dimensional border. The effect is commonly used for raised or indented buttons and similar graphics.

**Convolution** Distorting pixels in an image by combining each pixel’s value with the values of some or all of its neighboring pixels, using various ratios.

**Displacement** Shifting or moving pixels in an image to a new position.

**Matrix** A grid of numbers used to perform certain mathematical calculations by applying the numbers in the grid to various values, then combining the results.

**More Help topics**

[openfl.filters package](https://api.openfl.org/openfl/filters/index.html)
[openfl.display.DisplayObject.filters](https://api.openfl.org/openfl/display/DisplayObject.html#filters)
[openfl.display.BitmapData.applyFilter()](https://api.openfl.org/openfl/display/BitmapData.html#applyFilter)