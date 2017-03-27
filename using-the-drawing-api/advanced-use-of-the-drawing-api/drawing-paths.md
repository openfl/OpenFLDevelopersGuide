# Drawing Paths

The section on drawing lines and curves (see [Drawing lines and curves](../drawing-lines-and-curves.md)) introduced the commands for drawing a single line (`Graphics.lineTo()`) or curve (`Graphics.curveTo()`) and then moving the line to another point (`Graphics.moveTo()`) to form a shape. The [Graphics.drawPath()](http://api.openfl.org/openfl/display/Graphics.html#drawPath) and [Graphics.drawTriangles()](http://api.openfl.org/openfl/display/Graphics.html#drawTriangles) methods accept a set of objects representing those same drawing commands as a parameter. With these methods, you can provide a series of `Graphics.lineTo()`, `Graphics.curveTo()`, or `Graphics.moveTo()` commands for the compiled project to execute in a single statement.

The [GraphicsPathCommand](http://api.openfl.org/openfl/display/GraphicsPathCommand.html) enumeration class defines a set of constants that correspond to drawing commands. You pass a series of these constants (wrapped in a Vector instance) as a parameter for the `Graphics.drawPath()` method. Then with a single command you can render an entire shape, or several shapes. You can also alter the values passed to these methods to change an existing shape.

In addition to the Vector of drawing commands, the `drawPath()` method needs a set of coordinates that correspond to the coordinates for each drawing command. Create a Vector instance containing coordinates (Float values) and pass it to the `drawPath()` method as the second (data) argument.

**_Note:_ **_The values in the vector are not Point objects; the vector is a series of numbers where each group of two numbers represents an x/y coordinate pair._

The `Graphics.drawPath()` method matches each command with its respective point values (a collection of two or four numbers) to generate a path in the Graphics object:

```haxe
package;

import openfl.display.*;

class DrawPathExample extends Sprite {
	
	public function new () {
		
		var squareCommands = new Vector<Int> (5, true);
		squareCommands[0] = GraphicsPathCommand.MOVE_TO;
		squareCommands[1] = GraphicsPathCommand.LINE_TO;
		squareCommands[2] = GraphicsPathCommand.LINE_TO;
		squareCommands[3] = GraphicsPathCommand.LINE_TO;
		squareCommands[4] = GraphicsPathCommand.LINE_TO;
		
		var squareCoord = new Vector<Float> (10, true);
		squareCoord[0] = 20; //x
		squareCoord[1] = 10; //y squareCoord[2] = 50;
		squareCoord[3] = 10;
		squareCoord[4] = 50;
		squareCoord[5] = 40;
		squareCoord[6] = 20;
		squareCoord[7] = 40;
		squareCoord[8] = 20;
		squareCoord[9] = 10;
		graphics.beginFill (0x442266); //set the color
		graphics.drawPath (squareCommands, squareCoord);
		
	}
	
}
```