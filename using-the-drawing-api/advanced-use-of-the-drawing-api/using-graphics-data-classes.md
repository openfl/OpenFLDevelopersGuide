# Using graphics data classes {#using-graphics-data-classes}

The enhanced drawing API includes a set of classes in the openfl.display package that implement the [IGraphicsData](http://api.openfl.org/openfl/display/IGraphicsData.html) interface. These classes act as value objects (data containers) that represent the drawing methods of the drawing API.

The following classes implement the IGraphicsData interface:

*   GraphicsBitmapFill
*   GraphicsEndFill
*   GraphicsGradientFill
*   GraphicsPath
*   GraphicsShaderFill
*   GraphicsSolidFill
*   GraphicsStroke
*   GraphicsTrianglePath

With these classes, you can store a complete drawing in a Vector object of IGraphicsData type (Vector<IGraphicsData>). You can then reuse the graphics data as the data source for other shape instances or to store drawing information for later use.

Notice you have multiple fill classes for each style of fill, but only one stroke class. OpenFL has only one stroke IGraphicsData class because the stroke class uses the fill classes to define its style. So every stroke is actually defined by a combination of the stroke class and a fill class. Otherwise, the API for these graphics data classes mirror the methods they represent in the openfl.display.Graphics class:

| **Graphics Method** | **Corresponding Class** |
| --- | --- |
| `beginBitmapFill()` | GraphicsBitmapFill |
| `beginFill()` | GraphicsSolidFill |
| `beginGradientFill()` | GraphicsGradientFill |
| `beginShaderFill()` | GraphicsShaderFill |
| `lineBitmapStyle()` | GraphicsStroke + GraphicsBitmapFill |
| `lineGradientStyle()` | GraphicsStroke + GraphicsGradientFill |
| `lineShaderStyle()` | GraphicsStroke + GraphicsShaderFill |
| `lineStyle()` | GraphicsStroke + GraphicsSolidFill |
| `moveTo()`<br/>`lineTo()`<br/>`curveTo()`<br/>`drawPath()` | GraphicsPath |
| `drawTriangles()` | GraphicsTrianglePath |

In addition, the [GraphicsPath class](http://api.openfl.org/openfl/display/GraphicsPath.html) has its own `GraphicsPath.moveTo()`, `GraphicsPath.lineTo()`, `GraphicsPath.curveTo()`, `GraphicsPath.wideLineTo()`, and `GraphicsPath.wideMoveTo()` utility methods to easily define those commands for a GraphicsPath instance. These utility methods simplify the task of defining or updating the commands and data values directly.

## Drawing with vector graphics data

Once you have a collection of IGraphicsData instances, use the Graphics class’s `drawGraphicsData()` method to render the graphics. The `drawGraphicsData()` method carries out a set of drawing instructions from a vector of IGraphicsData instances in sequential order:

```haxe
// stroke object
var stroke = new GraphicsStroke (3);
stroke.joints = JointStyle.MITER;
stroke.fill = new GraphicsSolidFill (0x102020); // solid stroke

// fill object
var fill = new GraphicsGradientFill ();
fill.colors = [0x0000FF, 0xEEFFEE];
fill.matrix = new Matrix ();
fill.matrix.createGradientBox (70, 70, Math.PI/2);

// path object
var path = new GraphicsPath (new Vector<Int> (), new Vector<Float> ());
path.commands.push (GraphicsPathCommand.MOVE_TO, GraphicsPathCommand.LINE_TO, GraphicsPathCommand.LINE_TO);
path.data.push (125, 0, 50, 100, 175, 0);

// combine objects for complete drawing
var drawing = new Vector<IGraphicsData> ();
drawing.push (stroke, fill, path);

// draw the drawing graphics.drawGraphicsData (drawing);
```

By modifying one value in the path used by the drawing in the example, the shape can be redrawn multiple times for a more complex image:

```haxe
// draw the drawing multiple times
// change one value to modify each variation
graphics.drawGraphicsData (drawing);
path.data[2] += 200;
graphics.drawGraphicsData (drawing);
path.data[2] -= 150;
graphics.drawGraphicsData (drawing);
path.data[2] += 100;
graphics.drawGraphicsData (drawing);
path.data[2] -= 50;
graphicsS.drawGraphicsData (drawing);
```

Though IGraphicsData objects can define fill and stroke styles, the fill and stroke styles are not a requirement. In other words, Graphics class methods can be used to set styles while IGraphicsData objects can be used to draw a saved collection of paths, or vice-versa.

**_Note:_** _Use the `Graphics.clear()` method to clear out a previous drawing before starting a new one; unless you&#039;re adding on to the original drawing, as seen in the example above. As you change a single portion of a path or collection of IGraphicsData objects, redraw the entire drawing to see the changes._

When using graphics data classes, the fill is rendered whenever three or more points are drawn, because the shape is inherently closed at that point. Even though the fill closes, the stroke does not, and this behavior is different than when using multiple `Graphics.lineTo()` or `Graphics.moveTo()` commands.

## Reading vector graphics data

In addition to drawing vector content to a display object, you can use the Graphics class’s `readGraphicsData()` method to obtain a data representation of the vector graphics content of a display object. This can be used to create a snapshot of a graphic to save, copy, create a spritesheet at run time, and more.

Calling the `readGraphicsData()` method returns a Vector instance containing IGraphicsData objects. These are the same objects used to draw vector graphics with the `drawGraphicsData()` method.

There are several limitations to reading vector graphics with the `readGraphicsData()` method. For more information, see the [readGraphicsData() entry in the API Reference](http://api.openfl.org/openfl/display/Graphics.html#readGraphicsData).