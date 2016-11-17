## Creating MovieClip objects with ActionScript {#creating-movieclip-objects-with-actionscript}

Flash Player 9 and later, Adobe AIR 1.0 and later

One way of adding content to the screen in Flash is by dragging assets from the library onto the Stage, but that is not the only workflow. For complex projects, experienced developers commonly prefer to create movie clips programatically. This approach brings several advantages: easier re-use of code, faster compile-time speed, and more sophisticated modifications that are available only to ActionScript.

The display list API of ActionScript 3.0 streamlines the process of dynamically creating MovieClip objects. The ability to instantiate a MovieClip instance directly, separate from the process of adding it to the display list, provides flexibility and simplicity without sacrificing control.

In ActionScript 3.0, when you create a movie clip (or any other display object) instance programatically, it is not visible on the screen until it is added to the display list by calling the addChild() or the addChildAt() method on a display object container. This allows you to create a movie clip, set its properties, and even call methods before it is rendered to the screen. For more information on working with the display list, see

“Working with display object containers” on

page 159

.

**Exporting library symbols for ActionScript**

Flash Player 9 and later, Adobe AIR 1.0 and later

By default, instances of movie clip symbols in a Flash document’s library cannot be dynamically created (that is, created using only ActionScript). This is because each symbol that is exported for use in ActionScript adds to the size of your SWF file, and it’s recognized that some symbols might not be intended for use on the stage. For this reason, in order to make a symbol available in ActionScript, you must specify that the symbol should be exported for ActionScript.

To export a symbol for ActionScript:

1.  Select the symbol in the Library panel and open its Symbol Properties dialog box.
2.  If necessary, activate the Advanced settings.
3.  In the Linkage section, activate the Export for ActionScript checkbox. This will activate the Class and Base Class fields.

By default, the Class field is populated with the symbol name, with spaces removed (for example, a symbol named “Tree House” would become “TreeHouse”). To specify that the symbol should use a custom class for its behavior, enter the full name of the class including its package in this field. If you want to be able to create instances of the symbol in ActionScript, but don’t need to add any additional behavior, you can leave the class name as-is.

The Base Class field’s value defaults to flash.display.MovieClip. If you want your symbol to extend the functionality of another customer class, you can specify that class’s name instead, as long as that class extends the Sprite (or MovieClip) class.

1.  Press the OK button to save the changes.

At this point, if Flash can’t find a linked SWC file or an external ActionScript file with a definition for the specified class (for instance, if you didn’t need to add additional behavior for the symbol), a warning is displayed:

_A definition for this class could not be found in the classpath, so one will be automatically generated in the SWF file upon export_.

You can disregard this warning if your library symbol does not require unique functionality beyond the functionality of the MovieClip class.

If you do not provide a class for your symbol, Flash will create a class for your symbol equivalent to this one:

package

{

import flash.display.MovieClip;

public class ExampleMovieClip extends MovieClip

{

public function ExampleMovieClip()

{

}

}

}

If you do want to add extra ActionScript functionality to your symbol, add the appropriate properties and methods to the code structure below. For example, suppose you have a movie clip symbol containing a circle of 50 pixels width and 50 pixels height, and the symbol is specified to be exported for ActionScript with a class named Circle. The following code, when placed in a Circle.as file, extends the MovieClip class and provides the symbol with the additional methods getArea() and getCircumference():

package

{

import flash.display.MovieClip;

public class Circle extends MovieClip

{

public function Circle()

{

}

public function getArea():Number

{

// The formula is Pi times the radius squared. return Math.PI * Math.pow((width / 2), 2);

}

public function getCircumference():Number

{

// The formula is Pi times the diameter. return Math.PI * width;

}

}

}

The following code, placed on a keyframe on Frame 1 of the Flash document, will create an instance of the symbol and display it on the screen:

var c:Circle = new Circle(); addChild(c);

trace(c.width); trace(c.height); trace(c.getArea()); trace(c.getCircumference());

This code demonstrates ActionScript-based instantiation as an alternative to dragging individual assets onto the Stage. It creates a circle that has all of the properties of a movie clip and also has the custom methods defined in the Circle class. This is a very basic example—your library symbol can specify any number of properties and methods in its class.

ActionScript-based instantiation is powerful, because it allows you to dynamically create large quantities of instances that would be tedious to arrange manually. It is also flexible, because you can customize each instance’s properties as it is created. You can get a sense of both of these benefits by using a loop to dynamically create several Circle instances. With the Circle symbol and class described previously in your Flash document’s library, place the following code on a keyframe on Frame 1:

import flash.geom.ColorTransform;

var totalCircles:uint = 10; var i:uint;

for (i = 0; i &lt; totalCircles; i++)

{

// Create a new Circle instance. var c:Circle = new Circle();

// Place the new Circle at an x coordinate that will space the circles

// evenly across the Stage.

c.x = (stage.stageWidth / totalCircles) * i;

// Place the Circle instance at the vertical center of the Stage.

c.y = stage.stageHeight / 2;

// Change the Circle instance to a random color c.transform.colorTransform = getRandomColor();

// Add the Circle instance to the current timeline. addChild(c);

}

function getRandomColor():ColorTransform

{

// Generate random values for the red, green, and blue color channels. var red:Number = (Math.random() * 512) - 255;

var green:Number = (Math.random() * 512) - 255; var blue:Number = (Math.random() * 512) - 255;

// Create and return a ColorTransform object with the random colors. return new ColorTransform(1, 1, 1, 1, red, green, blue, 0);

}

This demonstrates how you can create and customize multiple instances of a symbol quickly using code. Each instance is positioned based on the current count within the loop, and each instance is given a random color by setting its transform property (which Circle inherits by extending the MovieClip class).