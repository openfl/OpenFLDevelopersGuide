# Changing position

The most basic manipulation to any display object is positioning it on the screen. To set a display object’s position, change the object’s `x` and `y` properties.

```haxe
myShape.x = 17;
myShape.y = 212;
```

The display object positioning system treats the Stage as a Cartesian coordinate system (the common grid system with a horizontal x axis and vertical y axis). The origin of the coordinate system (the 0,0 coordinate where the x and y axes meet) is at the top-left corner of the Stage. From there, x values are positive going right and negative going left, while (in contrast to typical graphing systems) y values are positive going down and negative going up. For example, the previous lines of code move the object `myShape` to the x coordinate 17 (17 pixels to the right of the origin) and y coordinate 212 (212 pixels below the origin).

By default, when a display object is created using Haxe, the `x` and `y` properties are both set to 0, placing the object at the top-left corner of its parent content.

### Changing position relative to the Stage

It’s important to remember that the `x` and `y` properties always refer to the position of the display object relative to the 0,0 coordinate of its parent display object’s axes. So for a Shape instance (such as a circle) contained inside a Sprite instance, setting the Shape object’s `x` and `y` properties to 0 will place the circle at the top-left corner of the Sprite, which is not necessarily the top-left corner of the Stage. To position an object relative to the global Stage coordinates, you can use the `globalToLocal()` method of any display object to convert coordinates from global (Stage) coordinates to local (display object container) coordinates, like this:

```haxe
// Position the shape at the top-left corner of the Stage,
// regardless of where its parent is located.

// Create a Sprite, positioned at x:200 and y:200.
var mySprite = new Sprite ();
mySprite.x = 200;
mySprite.y = 200;
this.addChild (mySprite);

// Draw a dot at the Sprite's 0,0 coordinate, for reference.
mySprite.graphics.lineStyle (1, 0x000000);
mySprite.graphics.beginFill (0x000000);
mySprite.graphics.moveTo (0, 0);
mySprite.graphics.lineTo (1, 0);
mySprite.graphics.lineTo (1, 1);
mySprite.graphics.lineTo (0, 1);
mySprite.graphics.endFill ();

// Create the circle Shape instance.
var circle = new Shape ();
mySprite.addChild (circle);

// Draw a circle with radius 50 and center point at x:50, y:50 in the Shape. circle.graphics.lineStyle (1, 0x000000);
circle.graphics.beginFill (0xff0000);
circle.graphics.drawCircle (50, 50, 50);
circle.graphics.endFill ();

// Move the Shape so its top-left corner is at the Stage's 0, 0 coordinate.
var stagePoint = new Point (0, 0);
var targetPoint = mySprite.globalToLocal (stagePoint);
circle.x = targetPoint.x;
circle.y = targetPoint.y;
```

You can likewise use the DisplayObject class’s `localToGlobal()` method to convert local coordinates to Stage coordinates.

### Moving display objects with the mouse

You can let a user move display objects with mouse using two different techniques in Haxe. In both cases, two mouse events are used: when the mouse button is pressed down, the object is told to follow the mouse cursor, and when it’s released, the object is told to stop following the mouse cursor.

<!--
**_Note:_** _OpenFL 11.3 and higher, AIR 3.3 and higher: You can also use the MouseEvent.RELEASE_OUTSIDE event to cover the case of a user releasing the mouse button outside the bounds of the containing Sprite._
-->

The first technique, using the `startDrag()` method, is simpler, but more limited. When the mouse button is pressed, the `startDrag()` method of the display object to be dragged is called. When the mouse button is released, the `stopDrag()` method is called. The Sprite class defines these two functions, so the object moved must be a Sprite or one of its subclasses.

```haxe
// This code creates a mouse drag interaction using the startDrag()
// technique.
// square is a MovieClip or Sprite instance.

import openfl.events.MouseEvent;

// This function is called when the mouse button is pressed.
private function startDragging (event:MouseEvent):Void {
	
	square.startDrag ();
	
}

// This function is called when the mouse button is released.
private function stopDragging (event:MouseEvent):Void {
	
	square.stopDrag ();
	
}

...

square.addEventListener (MouseEvent.MOUSE_DOWN, startDragging);
square.addEventListener (MouseEvent.MOUSE_UP, stopDragging);
```

This technique suffers from one fairly significant limitation: only one item at a time can be dragged using `startDrag()`. If one display object is being dragged and the `startDrag()` method is called on another display object, the first display object stops following the mouse immediately. For example, if the `startDragging()` function is changed as shown here, only the circle object will be dragged, in spite of the `square.startDrag()` method call:

```haxe
private function startDragging (event:MouseEvent):Void {
	
	square.startDrag ();
	circle.startDrag ();
	
}
```

As a consequence of the fact that only one object can be dragged at a time using `startDrag()`, the `stopDrag()` method can be called on any display object and it stops whatever object is currently being dragged.

If you need to drag more than one display object, or to avoid the possibility of conflicts where more than one object might potentially use `startDrag()`, it’s best to use the mouse-following technique to create the dragging effect. With this technique, when the mouse button is pressed, a function is subscribed as a listener to the `mouseMove` event of the Stage. This function, which is then called every time the mouse moves, causes the dragged object to jump to the x, y coordinate of the mouse. Once the mouse button is released, the function is unsubscribed as a listener, meaning it is no longer called when the mouse moves and the object stops following the mouse cursor. Here is some code that demonstrates this technique:

```haxe
// This code moves display objects using the mouse-following
// technique.
// circle is a DisplayObject (e.g. a MovieClip or Sprite instance).

import openfl.events.MouseEvent;

...

private var offsetX:Float;
private var offsetY:Float;

...

// This function is called when the mouse button is pressed.
private function startDragging (event:MouseEvent):Void {
	
	// Record the difference (offset) between where
	// the cursor was when the mouse button was pressed and the x, y
	// coordinate of the circle when the mouse button was pressed.
	offsetX = event.stageX - circle.x;
	offsetY = event.stageY - circle.y;
	
	// tell OpenFL to start listening for the mouseMove event
	stage.addEventListener (MouseEvent.MOUSE_MOVE, dragCircle);
	
}

// This function is called when the mouse button is released.
private function stopDragging (event:MouseEvent):Void {
	
	// Tell OpenFL to stop listening for the mouseMove event.
	stage.removeEventListener (MouseEvent.MOUSE_MOVE, dragCircle);
	
}

// This function is called every time the mouse moves,
// as long as the mouse button is pressed down.
private function dragCircle (event:MouseEvent):Void {
	
	// Move the circle to the location of the cursor, maintaining
	// the offset between the cursor's location and the
	// location of the dragged object.
	circle.x = event.stageX - offsetX;
	circle.y = event.stageY - offsetY;
	
	// Instruct OpenFL to refresh the screen after this event.
	event.updateAfterEvent ();
	
}

...

circle.addEventListener (MouseEvent.MOUSE_DOWN, startDragging);
circle.addEventListener (MouseEvent.MOUSE_UP, stopDragging);
```

In addition to making a display object follow the mouse cursor, it is often desirable to move the dragged object to the front of the display, so that it appears to be floating above all the other objects. For example, suppose you have two objects, a circle and a square, that can both be moved with the mouse. If the circle happens to be below the square on the display list, and you click and drag the circle so that the cursor is over the square, the circle will appear to slide behind the square, which breaks the drag-and-drop illusion. Instead, you can make it so that when the circle is clicked, it moves to the top of the display list, and thus always appears on top of any other content.

The following code (adapted from the previous example) allows two display objects, a circle and a square, to be moved with the mouse. Whenever the mouse button is pressed over either one, that item is moved to the top of the Stage’s display list, so that the dragged item always appears on top. <!-- (Code that is new or changed from the previous listing appears in boldface.) -->

```haxe
// This code moves display objects using the mouse-following
// technique.
// circle is a DisplayObject (e.g. a MovieClip or Sprite instance).

import openfl.display.DisplayObject;
import openfl.events.MouseEvent;

...

private var offsetX:Float;
private var offsetY:Float;
private var draggedObject:DisplayObject;

...

// This function is called when the mouse button is pressed.
private function startDragging (event:MouseEvent):Void {
	
	// remember which object is being dragged
	draggedObject = cast (event.target, DisplayObject);
	
	// Record the difference (offset) between where
	// the cursor was when the mouse button was pressed and the x, y
	// coordinate of the circle when the mouse button was pressed.
	offsetX = event.stageX - circle.x;
	offsetY = event.stageY - circle.y;
	
	// move the selected object to the top of the display list 
    stage.addChild (draggedObject);
	
	// tell OpenFL to start listening for the mouseMove event
	stage.addEventListener (MouseEvent.MOUSE_MOVE, dragCircle);
	
}

// This function is called when the mouse button is released.
private function stopDragging (event:MouseEvent):Void {
	
	// Tell OpenFL to stop listening for the mouseMove event.
	stage.removeEventListener (MouseEvent.MOUSE_MOVE, dragCircle);
	
}

// This function is called every time the mouse moves,
// as long as the mouse button is pressed down.
private function dragCircle (event:MouseEvent):Void {
	
	// Move the circle to the location of the cursor, maintaining
	// the offset between the cursor's location and the
	// location of the dragged object.
	circle.x = event.stageX - offsetX;
	circle.y = event.stageY - offsetY;
	
	// Instruct OpenFL to refresh the screen after this event.
	event.updateAfterEvent ();
	
}

...

circle.addEventListener (MouseEvent.MOUSE_DOWN, startDragging);
circle.addEventListener (MouseEvent.MOUSE_UP, stopDragging);

square.addEventListener (MouseEvent.MOUSE_DOWN, startDragging); 
square.addEventListener (MouseEvent.MOUSE_UP, stopDragging);
```

<!-- 
```haxe
// This code creates a drag-and-drop interaction using the mouse-following
// technique.
// circle and square are DisplayObjects (e.g. MovieClip or Sprite
// instances).

**import openfl.display.DisplayObject;**
import openfl.events.MouseEvent;

var offsetX:Number;
var offsetY:Number;

**var draggedObject:DisplayObject;**

// This function is called when the mouse button is pressed. function startDragging(event:MouseEvent):void

{

**// remember which object is being dragged draggedObject = DisplayObject(event.target);**

// Record the difference (offset) between where the cursor was when

// the mouse button was pressed and the x, y coordinate of the

// dragged object when the mouse button was pressed. offsetX = event.stageX - draggedObject.x;

offsetY = event.stageY - draggedObject.y;

**// move the selected object to the top of the display list stage.addChild(draggedObject);**

// Tell OpenFL to start listening for the mouseMove event. stage.addEventListener(MouseEvent.MOUSE_MOVE, dragObject);

}

// This function is called when the mouse button is released. function stopDragging(event:MouseEvent):void

{

// Tell OpenFL to stop listening for the mouseMove event. stage.removeEventListener(MouseEvent.MOUSE_MOVE, dragObject);

}

// This function is called every time the mouse moves,

// as long as the mouse button is pressed down. function dragObject(event:MouseEvent):void

{

// Move the dragged object to the location of the cursor, maintaining

// the offset between the cursor's location and the location

// of the dragged object.

draggedObject.x = event.stageX - offsetX; draggedObject.y = event.stageY - offsetY;

// Instruct OpenFL to refresh the screen after this event. event.updateAfterEvent();

}

circle.addEventListener(MouseEvent.MOUSE_DOWN, startDragging); circle.addEventListener(MouseEvent.MOUSE_UP, stopDragging);

**square.addEventListener(MouseEvent.MOUSE_DOWN, startDragging); square.addEventListener(MouseEvent.MOUSE_UP, stopDragging);**
-->

To extend this effect further, such as for a game where tokens or cards are moved among piles, you could add the dragged object to the Stage’s display list when it’s "picked up," and then add it to another display list—such as the "pile" where it is dropped—when the mouse button is released.

<!-- TODO: uncomment when the filtering chapter is available
Finally, to enhance the effect, you could apply a drop shadow filter to the display object when it is clicked (when you start dragging it) and remove the drop shadow when the object is released. For details on using the drop shadow filter and other display object filters in Haxe, see [Filtering display objects](/filtering-display-objects/README.md).-->