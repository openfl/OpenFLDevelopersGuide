# Using Point objects {#using-point-objects}

A [Point](http://api.openfl.org/openfl/geom/Point.html) object defines a Cartesian pair of coordinates. It represents location in a two-dimensional coordinate system, where _x_ represents the horizontal axis and _y_ represents the vertical axis.

To define a Point object, you set its `x` and `y` properties, as follows:

```haxe
import openfl.geom.*;

...

var pt1 = new Point (10, 20); // x == 10; y == 20
var pt2 = new Point ();
pt2.x = 10;
pt2.y = 20;
```

## Finding the distance between two points

You can use the `distance()` method of the Point class to find the distance between two points in a coordinate space. For example, the following code finds the distance between the registration points of two display objects, `circle1` and `circle2`, in the same display object container:

```haxe
import openfl.geom.*;

...

var pt1 = new Point (circle1.x, circle1.y);
var pt2 = new Point (circle2.x, circle2.y);
var distance = Point.distance (pt1, pt2);
```

## Translating coordinate spaces {#translating-coordinate-spaces}

If two display objects are in different display object containers, they can be in different coordinate spaces. You can use the `localToGlobal()` method of the DisplayObject class to translate the coordinates to the same (global) coordinate space, that of the Stage. For example, the following code finds the distance between the registration points of two display objects, `circle1` and `circle2`, in the different display object containers:

```haxe
import openfl.geom.*;

...

var pt1 = new Point (circle1.x, circle1.y);
pt1 = circle1.localToGlobal (pt1);
var pt2 = new Point (circle2.x, circle2.y);
pt2 = circle2.localToGlobal (pt2);
var distance = Point.distance (pt1, pt2);
```

Similarly, to find the distance of the registration point of a display object named `target` from a specific point on the Stage, use the `localToGlobal()` method of the DisplayObject class:

```haxe
import openfl.geom.*;

...

var stageCenter = new Point ();
stageCenter.x = this.stage.stageWidth / 2;
stageCenter.y = this.stage.stageHeight / 2;
var targetCenter = new Point (target.x, target.y);
targetCenter = target.localToGlobal (targetCenter);
var distance = Point.distance (stageCenter, targetCenter);
```

## Moving a display object by a specified angle and distance {#moving-a-display-object-by-a-specified-angle-and-distance}

You can use the `polar()` method of the Point class to move a display object a specific distance by a specific angle. For example, the following code moves the `myDisplayObject` object 100 pixels by 60°:

```haxe
import openfl.geom.*;

...

var distance = 100;
var angle = 2 * Math.PI * (90 / 360);
var translatePoint = Point.polar (distance, angle);
myDisplayObject.x += translatePoint.x;
myDisplayObject.y += translatePoint.y;
```

## Other uses of the Point class {#other-uses-of-the-point-class}

You can use Point objects with the following methods and properties:

| **Class** | **Methods or properties** | **Description** |
| --- | --- | --- |
| DisplayObjectContainer | `areInaccessibleObjectsUnderPoint()`<br/>`getObjectsUnderPoint()` | Used to return a list of objects under a point in a display object container. |
| BitmapData | `hitTest()` | Used to define the pixel in the BitmapData object as well as the point that you are checking for a hit. |
| BitmapData | `applyFilter()`<br/>`copyChannel()`<br/>`merge()`<br/>`paletteMap()`<br/>`pixelDissolve()`<br/>`threshold()` | Used to define the positions of rectangles that define the operations. |
| Matrix | `deltaTransformPoint()`<br/>`transformPoint()` | Used to define points for which you want to apply a transformation. |
| Rectangle | `bottomRight`<br/>`size`<br/>`topLeft` | Used to define these properties. |