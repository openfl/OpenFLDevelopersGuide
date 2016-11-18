## Advanced use of the drawing API {#advanced-use-of-the-drawing-api}

Flash Player 10 and later, Adobe AIR 1.5 and later

Flash Player 10, Adobe AIR 1.5, and later Flash runtimes, support an advanced set of drawing features. The drawing API enhancements for these runtimes expand upon the drawing methods from previous releases so you can establish data sets to generate shapes, alter shapes at runtime, and create three-dimensional effects. The drawing API enhancements consolidate existing methods into alternative commands. These commands leverage vector arrays and enumeration classes to provide data sets for drawing methods. Using vector arrays allows for more complex shapes to render quickly and for developers to change the array values programmatically for dynamic shape rendering at runtime.

The drawing features introduced in Flash Player 10 are described in the following sections:

“Drawing Paths” on

page 236

,

“Defining winding rules” on page 237

,

“Using graphics data classes” on page 239

, and

“About using

drawTriangles()” on page 241

.

The following tasks are things you’ll likely want to accomplish using the advanced drawing API in ActionScript:

*   Using Vector objects to store data for drawing methods
*   Defining paths for drawing shapes programmatically in a single operation
*   Defining winding rules to determine how overlapping shapes are filled
*   Reading the vector graphics content of a display object, such as to serialize and save the graphics data, to generate a spritesheet at runtime, or to draw a copy of the vector graphics content
*   Using triangles and drawing methods for three-dimensional effects

Important concepts and terms

The following reference list contains important terms that you will encounter in this section:

*   Vector: An array of values all of the same data type. A Vector object can store an array of values that drawing methods use to construct lines and shapes with a single command. For more information on Vector objects, see

    “Indexed arrays” on page 26

    .
*   Path: A path is made up of one or more straight or curved segments. The beginning and end of each segment are marked by coordinates, which work like pins holding a wire in place. A path can be closed (for example, a circle), or open, with distinct endpoints (for example, a wavy line).
*   Winding: The direction of a path as interpreted by the renderer; either positive (clockwise) or negative (counter- clockwise).
*   GraphicsStroke: A class for setting the line style. While the term “stroke” isn’t part of the drawing API enhancements, the use of a class to designate a line style with its own fill property is part of the new drawing API. You can dynamically adjust a line’s style using the GraphicsStroke class.
*   Fill object: Objects created using display classes like flash.display.GraphicsBitmapFill and flash.display.GraphicsGradientFill that are passed to the drawing command Graphics.drawGraphicsData(). Fill objects and the enhanced drawing commands introduce a more object-oriented programming approach to replicating Graphics.beginBitmapFill() and Graphics.beginGradientFill().

**Drawing Paths**

Flash Player 10 and later, Adobe AIR 1.5 and later

The section on drawing lines and curves (see

“Drawing lines and curves” on page 223

) introduced the commands for drawing a single line (Graphics.lineTo()) or curve (Graphics.curveTo()) and then moving the line to another point (Graphics.moveTo()) to form a shape. The [Graphics.drawPath()](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Graphics.html#drawPath()) and [Graphics.drawTriangles()](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Graphics.html#drawTriangles()) methods accept a set of objects representing those same drawing commands as a parameter. With these methods, you can provide a series of Graphics.lineTo(), Graphics.curveTo(), or Graphics.moveTo() commands for the Flash runtime to execute in a single statement.

The [GraphicsPathCommand](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/GraphicsPathCommand.html) enumeration class defines a set of constants that correspond to drawing commands. You pass a series of these constants (wrapped in a Vector instance) as a parameter for the Graphics.drawPath() method. Then with a single command you can render an entire shape, or several shapes. You can also alter the values passed to these methods to change an existing shape.

In addition to the Vector of drawing commands, the drawPath() method needs a set of coordinates that correspond to the coordinates for each drawing command. Create a Vector instance containing coordinates (Number instances) and pass it to the drawPath() method as the second (data) argument.

**_Note:_ **_The values in the vector are not Point objects; the vector is a series of numbers where each group of two numbers represents an x/y coordinate pair._

The Graphics.drawPath() method matches each command with its respective point values (a collection of two or four numbers) to generate a path in the Graphics object:

package

{

import flash.display.*;

public class DrawPathExample extends Sprite

{

public function DrawPathExample(){

var squareCommands:Vector.&lt;int&gt; = new Vector.&lt;int&gt;(5, true); squareCommands[0] = GraphicsPathCommand.MOVE_TO; squareCommands[1] = GraphicsPathCommand.LINE_TO; squareCommands[2] = GraphicsPathCommand.LINE_TO; squareCommands[3] = GraphicsPathCommand.LINE_TO; squareCommands[4] = GraphicsPathCommand.LINE_TO;

var squareCoord:Vector.&lt;Number&gt; = new Vector.&lt;Number&gt;(10, true); squareCoord[0] = 20; //x

squareCoord[1] = 10; //y squareCoord[2] = 50;

squareCoord[3] = 10;

squareCoord[4] = 50;

squareCoord[5] = 40;

squareCoord[6] = 20;

squareCoord[7] = 40;

squareCoord[8] = 20;

squareCoord[9] = 10;

graphics.beginFill(0x442266);//set the color graphics.drawPath(squareCommands, squareCoord);

}

}

}

### Defining winding rules {#defining-winding-rules}

Flash Player 10 and later, Adobe AIR 1.5 and later

The enhanced drawing API also introduces the concept of path “winding”: the direction for a path. The winding for a path is either positive (clockwise) or negative (counter-clockwise). The order in which the renderer interprets the coordinates provided by the vector for the data parameter determines the winding.

**A**

*   1.  **C**

_Positive and negative winding_

**_A._ **_Arrows indicate drawing direction_ **_B._ **_Positively wound (clockwise)_ **_C._ **_Negatively wound (counter-clockwise)_

Additionally, notice that the Graphics.drawPath() method has an optional third parameter called “winding”:

drawPath(commands:Vector.&lt;int&gt;, data:Vector.&lt;Number&gt;, winding:String = &quot;evenOdd&quot;):void

In this context, the third parameter is a string or a constant that specifies the winding or fill rule for intersecting paths. (The constant values are defined in the GraphicsPathWinding class as GraphicsPathWinding.EVEN_ODD or GraphicsPathWinding.NON_ZERO.) The winding rule is important when paths intersect.

The even-odd rule is the standard winding rule and is the rule used by the legacy drawing API. The Even-odd rule is also the default rule for the Graphics.drawPath() method. With the even-odd winding rule, any intersecting paths alternate between open and closed fills. If two squares drawn with the same fill intersect, the area in which the intersection occurs is filled. Generally, adjacent areas are neither both filled nor both unfilled.

The non-zero rule, on the other hand, depends on winding (drawing direction) to determine whether areas defined by intersecting paths are filled. When paths of opposite winding intersect, the area defined is unfilled, much like with even-odd. For paths of the same winding, the area that would be unfilled is filled:

![Winding rules for intersecting areas](C:\Development\Books\HaxeDevelopersGuide\export\assets\winding_rules_for_intersecting_area.png)

**A B**

_Winding rules for intersecting areas_

**_A._ **_Even-odd winding rule_ **_B._ **_Non-zero winding rule_

**Winding rule names**

Flash Player 10 and later, Adobe AIR 1.5 and later

The names refer to a more specific rule that defines how these fills are managed. Positively wound paths are assigned a value of +1; negatively wound paths are assigned a value of -1\. Starting from a point within an enclosed area of a shape, draw a line from that point extending out indefinitely. The number of times that line crosses a path, and the combined values of those paths, are used to determine the fill. For even-odd winding, the count of times the line crosses a path is used. When the count is odd, the area is filled. For even counts, the area is unfilled. For non-zero winding, the values assigned to the paths are used. When the combined values of the path are not 0, the area is filled. When the combined value is 0, the area is unfilled.

![Winding rule counts and fills](C:\Development\Books\HaxeDevelopersGuide\export\assets\winding_rule_counts_and_fills.png)

**A B**

_Winding rule counts and fills_

**_A._ **_Even-odd winding rule_ **_B._ **_Non-zero winding rule_

Using winding rules

Flash Player 10 and later, Adobe AIR 1.5 and later

These fill rules are complicated, but in some situations they are necessary. For example, consider drawing a star shape. With the standard even-odd rule, the shape would require ten different lines. With the non-zero winding rule, those ten lines are reduced to five. Here is the ActionScript for a star with five lines and a non-zero winding rule:

graphics.beginFill(0x60A0FF);

graphics.drawPath( Vector.&lt;int&gt;([1,2,2,2,2]), Vector.&lt;Number&gt;([66,10, 23,127, 122,50, 10,49,

109,127]), GraphicsPathWinding.NON_ZERO);

And here is the star shape:

![A star shape using different winding](C:\Development\Books\HaxeDevelopersGuide\export\assets\a_star_shape_using_different_windin.jpeg)

**A B C**

_A star shape using different winding rules_

**_A._ **_Even-odd 10 lines_ **_B._ **_Even-odd 5 lines_ **_C._ **_Non-zero 5 lines_

And, as images are animated or used as textures on three-dimensional objects and overlap, the winding rules become more important.

### Using graphics data classes {#using-graphics-data-classes}

Flash Player 10 and later, Adobe AIR 1.5 and later

The enhanced drawing API includes a set of classes in the flash.display package that implement the [IGraphicsData](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/IGraphicsData.html) interface. These classes act as value objects (data containers) that represent the drawing methods of the drawing API.

The following classes implement the IGraphicsData interface:

*   GraphicsBitmapFill
*   GraphicsEndFill
*   GraphicsGradientFill
*   GraphicsPath
*   GraphicsShaderFill
*   GraphicsSolidFill
*   GraphicsStroke
*   GraphicsTrianglePath

With these classes, you can store a complete drawing in a Vector object of IGraphicsData type (Vector.&lt;IGraphicsData&gt;). You can then reuse the graphics data as the data source for other shape instances or to store drawing information for later use.

Notice you have multiple fill classes for each style of fill, but only one stroke class. ActionScript has only one stroke IGraphicsData class because the stroke class uses the fill classes to define its style. So every stroke is actually defined by a combination of the stroke class and a fill class. Otherwise, the API for these graphics data classes mirror the methods they represent in the flash.display.Graphics class:

| **Graphics Method** | **Corresponding Class** |
| --- | --- |
| beginBitmapFill() | GraphicsBitmapFill |
| beginFill() | GraphicsSolidFill |
| beginGradientFill() | GraphicsGradientFill |
| beginShaderFill() | GraphicsShaderFill |
| lineBitmapStyle() | GraphicsStroke + GraphicsBitmapFill |
| lineGradientStyle() | GraphicsStroke + GraphicsGradientFill |
| lineShaderStyle() | GraphicsStroke + GraphicsShaderFill |
| lineStyle() | GraphicsStroke + GraphicsSolidFill |
| moveTo() lineTo() curveTo() drawPath() | GraphicsPath |
| drawTriangles() | GraphicsTrianglePath |

In addition, the [GraphicsPath class](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/GraphicsPath.html) has its own GraphicsPath.moveTo(), GraphicsPath.lineTo(), GraphicsPath.curveTo(), GraphicsPath.wideLineTo(), and GraphicsPath.wideMoveTo() utility methods to easily define those commands for a GraphicsPath instance. These utility methods simplify the task of defining or updating the commands and data values directly.

**Drawing with vector graphics data**

Once you have a collection of IGraphicsData instances, use the Graphics class’s drawGraphicsData() method to render the graphics. The drawGraphicsData() method carries out a set of drawing instructions from a vector of IGraphicsData instances in sequential order:

// stroke object

var stroke:GraphicsStroke = new GraphicsStroke(3); stroke.joints = JointStyle.MITER;

stroke.fill = new GraphicsSolidFill(0x102020);// solid stroke

// fill object

var fill:GraphicsGradientFill = new GraphicsGradientFill(); fill.colors = [0x0000FF, 0xEEFFEE];

fill.matrix = new Matrix(); fill.matrix.createGradientBox(70, 70, Math.PI/2);

// path object

var path:GraphicsPath = new GraphicsPath(new Vector.&lt;int&gt;(), new Vector.&lt;Number&gt;()); path.commands.push(GraphicsPathCommand.MOVE_TO, GraphicsPathCommand.LINE_TO, GraphicsPathCommand.LINE_TO);

path.data.push(125,0, 50,100, 175,0);

// combine objects for complete drawing

var drawing:Vector.&lt;IGraphicsData&gt; = new Vector.&lt;IGraphicsData&gt;(); drawing.push(stroke, fill, path);

// draw the drawing graphics.drawGraphicsData(drawing);

By modifying one value in the path used by the drawing in the example, the shape can be redrawn multiple times for a more complex image:

// draw the drawing multiple times

// change one value to modify each variation graphics.drawGraphicsData(drawing); path.data[2] += 200; graphics.drawGraphicsData(drawing); path.data[2] -= 150; graphics.drawGraphicsData(drawing); path.data[2] += 100; graphics.drawGraphicsData(drawing);

path.data[2] -= 50;graphicsS.drawGraphicsData(drawing);

Though IGraphicsData objects can define fill and stroke styles, the fill and stroke styles are not a requirement. In other words, Graphics class methods can be used to set styles while IGraphicsData objects can be used to draw a saved collection of paths, or vice-versa.

**_Note:_ **_Use the Graphics.clear() method to clear out a previous drawing before starting a new one; unless you&#039;re adding on to the original drawing, as seen in the example above. As you change a single portion of a path or collection of IGraphicsData objects, redraw the entire drawing to see the changes._

When using graphics data classes, the fill is rendered whenever three or more points are drawn, because the shape is inherently closed at that point. Even though the fill closes, the stroke does not, and this behavior is different than when using multiple Graphics.lineTo() or Graphics.moveTo() commands.

Reading vector graphics data

Flash Player 11.6 and later, Adobe AIR 3.6 and later

In addition to drawing vector content to a display object, in Flash Player 11.6 and Adobe AIR 3.6 and later you can use the Graphics class’s readGraphicsData() method to obtain a data representation of the vector graphics content of a display object. This can be used to create a snapshot of a graphic to save, copy, create a spritesheet at run time, and more.

Calling the readGraphicsData() method returns a Vector instance containing IGraphicsData objects. These are the same objects used to draw vector graphics with the drawGraphicsData() method.

There are several limitations to reading vector graphics with the readGraphicsData() method. For more information, see the [readGraphicsData() entry in the ActionScript Language Reference](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Graphics.html#readGraphicsData()).

### About using drawTriangles() {#about-using-drawtriangles}

Flash Player 10 and later, Adobe AIR 1.5 and later

Another advanced method introduced in Flash Player 10 and Adobe AIR 1.5, [Graphics.drawTriangles()](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Graphics.html#drawTriangles()), is like the [Graphics.drawPath()](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Graphics.html#drawPath()) method. The Graphics.drawTriangles() method also uses a Vector.&lt;Number&gt; object to specify point locations for drawing a path.

However, the real purpose for the Graphics.drawTriangles() method is to facilitate three-dimensional effects through ActionScript. For information about using Graphics.drawTriangles() to produce three-dimensional effects, see

“Using triangles for 3D effects” on page 363

.