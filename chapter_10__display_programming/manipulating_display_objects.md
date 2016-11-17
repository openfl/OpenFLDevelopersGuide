## Manipulating display objects {#manipulating-display-objects}

Flash Player 9 and later, Adobe AIR 1.0 and later

Regardless of which display object you choose to use, there are a number of manipulations that all display objects have in common as elements that are displayed on the screen. For example, they can all be positioned on the screen, moved forward or backward in the stacking order of display objects, scaled, rotated, and so forth. Because all display objects inherit this functionality from their common base class (DisplayObject), this functionality behaves the same whether you’re manipulating a TextField instance, a Video instance, a Shape instance, or any other display object. The following sections detail several of these common display object manipulations.

**Changing position**

Flash Player 9 and later, Adobe AIR 1.0 and later

The most basic manipulation to any display object is positioning it on the screen. To set a display object’s position, change the object’s x and y properties.

myShape.x = 17;

myShape.y = 212;

The display object positioning system treats the Stage as a Cartesian coordinate system (the common grid system with a horizontal x axis and vertical y axis). The origin of the coordinate system (the 0,0 coordinate where the x and y axes meet) is at the top-left corner of the Stage. From there, x values are positive going right and negative going left, while (in contrast to typical graphing systems) y values are positive going down and negative going up. For example, the previous lines of code move the object myShape to the x coordinate 17 (17 pixels to the right of the origin) and y coordinate 212 (212 pixels below the origin).

By default, when a display object is created using ActionScript, the x and y properties are both set to 0, placing the object at the top-left corner of its parent content.

**Changing position relative to the Stage**

It’s important to remember that the x and y properties always refer to the position of the display object relative to the 0,0 coordinate of its parent display object’s axes. So for a Shape instance (such as a circle) contained inside a Sprite instance, setting the Shape object’s x and y properties to 0 will place the circle at the top-left corner of the Sprite, which is not necessarily the top-left corner of the Stage. To position an object relative to the global Stage coordinates, you can use the globalToLocal() method of any display object to convert coordinates from global (Stage) coordinates to local (display object container) coordinates, like this:

// Position the shape at the top-left corner of the Stage,

// regardless of where its parent is located.

// Create a Sprite, positioned at x:200 and y:200\. var mySprite:Sprite = new Sprite();

mySprite.x = 200;

mySprite.y = 200; this.addChild(mySprite);

// Draw a dot at the Sprite&#039;s 0,0 coordinate, for reference. mySprite.graphics.lineStyle(1, 0x000000); mySprite.graphics.beginFill(0x000000); mySprite.graphics.moveTo(0, 0);

mySprite.graphics.lineTo(1, 0);

mySprite.graphics.lineTo(1, 1);

mySprite.graphics.lineTo(0, 1); mySprite.graphics.endFill();

// Create the circle Shape instance. var circle:Shape = new Shape(); mySprite.addChild(circle);

// Draw a circle with radius 50 and center point at x:50, y:50 in the Shape. circle.graphics.lineStyle(1, 0x000000);

circle.graphics.beginFill(0xff0000); circle.graphics.drawCircle(50, 50, 50); circle.graphics.endFill();

// Move the Shape so its top-left corner is at the Stage&#039;s 0, 0 coordinate. var stagePoint:Point = new Point(0, 0);

var targetPoint:Point = mySprite.globalToLocal(stagePoint); circle.x = targetPoint.x;

circle.y = targetPoint.y;

You can likewise use the DisplayObject class’s localToGlobal() method to convert local coordinates to Stage coordinates.

Moving display objects with the mouse

You can let a user move display objects with mouse using two different techniques in ActionScript. In both cases, two mouse events are used: when the mouse button is pressed down, the object is told to follow the mouse cursor, and when it’s released, the object is told to stop following the mouse cursor.

**_Note:_ **_Flash Player 11.3 and higher, AIR 3.3 and higher: You can also use the MouseEvent.RELEASE_OUTSIDE event to cover the case of a user releasing the mouse button outside the bounds of the containing Sprite._

The first technique, using the startDrag() method, is simpler, but more limited. When the mouse button is pressed, the startDrag() method of the display object to be dragged is called. When the mouse button is released, the stopDrag() method is called. The Sprite class defines these two functions, so the object moved must be a Sprite or one of its subclasses.

// This code creates a mouse drag interaction using the startDrag()

// technique.

// square is a MovieClip or Sprite instance). import flash.events.MouseEvent;

// This function is called when the mouse button is pressed. function startDragging(event:MouseEvent):void

{

square.startDrag();

}

// This function is called when the mouse button is released. function stopDragging(event:MouseEvent):void

{

square.stopDrag();

}

square.addEventListener(MouseEvent.MOUSE_DOWN, startDragging); square.addEventListener(MouseEvent.MOUSE_UP, stopDragging);

This technique suffers from one fairly significant limitation: only one item at a time can be dragged using startDrag(). If one display object is being dragged and the startDrag() method is called on another display object, the first display object stops following the mouse immediately. For example, if the startDragging() function is changed as shown here, only the circle object will be dragged, in spite of the square.startDrag() method call:

function startDragging(event:MouseEvent):void

{

square.startDrag(); circle.startDrag();

}

As a consequence of the fact that only one object can be dragged at a time using startDrag(), the stopDrag()

method can be called on any display object and it stops whatever object is currently being dragged.

If you need to drag more than one display object, or to avoid the possibility of conflicts where more than one object might potentially use startDrag(), it’s best to use the mouse-following technique to create the dragging effect. With this technique, when the mouse button is pressed, a function is subscribed as a listener to the mouseMove event of the Stage. This function, which is then called every time the mouse moves, causes the dragged object to jump to the x, y coordinate of the mouse. Once the mouse button is released, the function is unsubscribed as a listener, meaning it is no longer called when the mouse moves and the object stops following the mouse cursor. Here is some code that demonstrates this technique:

// This code moves display objects using the mouse-following

// technique.

// circle is a DisplayObject (e.g. a MovieClip or Sprite instance). import flash.events.MouseEvent;

var offsetX:Number; var offsetY:Number;

// This function is called when the mouse button is pressed. function startDragging(event:MouseEvent):void

{

// Record the difference (offset) between where

// the cursor was when the mouse button was pressed and the x, y

// coordinate of the circle when the mouse button was pressed. offsetX = event.stageX - circle.x;

offsetY = event.stageY - circle.y;

// tell Flash Player to start listening for the mouseMove event stage.addEventListener(MouseEvent.MOUSE_MOVE, dragCircle);

}

// This function is called when the mouse button is released. function stopDragging(event:MouseEvent):void

{

// Tell Flash Player to stop listening for the mouseMove event. stage.removeEventListener(MouseEvent.MOUSE_MOVE, dragCircle);

}

// This function is called every time the mouse moves,

// as long as the mouse button is pressed down. function dragCircle(event:MouseEvent):void

{

// Move the circle to the location of the cursor, maintaining

// the offset between the cursor&#039;s location and the

// location of the dragged object. circle.x = event.stageX - offsetX; circle.y = event.stageY - offsetY;

// Instruct Flash Player to refresh the screen after this event. event.updateAfterEvent();

}

circle.addEventListener(MouseEvent.MOUSE_DOWN, startDragging); circle.addEventListener(MouseEvent.MOUSE_UP, stopDragging);

In addition to making a display object follow the mouse cursor, it is often desirable to move the dragged object to the front of the display, so that it appears to be floating above all the other objects. For example, suppose you have two objects, a circle and a square, that can both be moved with the mouse. If the circle happens to be below the square on the display list, and you click and drag the circle so that the cursor is over the square, the circle will appear to slide behind the square, which breaks the drag-and-drop illusion. Instead, you can make it so that when the circle is clicked, it moves to the top of the display list, and thus always appears on top of any other content.

The following code (adapted from the previous example) allows two display objects, a circle and a square, to be moved with the mouse. Whenever the mouse button is pressed over either one, that item is moved to the top of the Stage’s display list, so that the dragged item always appears on top. (Code that is new or changed from the previous listing appears in boldface.)

// This code creates a drag-and-drop interaction using the mouse-following

// technique.

// circle and square are DisplayObjects (e.g. MovieClip or Sprite

// instances).

**import flash.display.DisplayObject;**

import flash.events.MouseEvent;

var offsetX:Number; var offsetY:Number;

**var draggedObject:DisplayObject;**

// This function is called when the mouse button is pressed. function startDragging(event:MouseEvent):void

{

**// remember which object is being dragged draggedObject = DisplayObject(event.target);**

// Record the difference (offset) between where the cursor was when

// the mouse button was pressed and the x, y coordinate of the

// dragged object when the mouse button was pressed. offsetX = event.stageX - draggedObject.x;

offsetY = event.stageY - draggedObject.y;

**// move the selected object to the top of the display list stage.addChild(draggedObject);**

// Tell Flash Player to start listening for the mouseMove event. stage.addEventListener(MouseEvent.MOUSE_MOVE, dragObject);

}

// This function is called when the mouse button is released. function stopDragging(event:MouseEvent):void

{

// Tell Flash Player to stop listening for the mouseMove event. stage.removeEventListener(MouseEvent.MOUSE_MOVE, dragObject);

}

// This function is called every time the mouse moves,

// as long as the mouse button is pressed down. function dragObject(event:MouseEvent):void

{

// Move the dragged object to the location of the cursor, maintaining

// the offset between the cursor&#039;s location and the location

// of the dragged object.

draggedObject.x = event.stageX - offsetX; draggedObject.y = event.stageY - offsetY;

// Instruct Flash Player to refresh the screen after this event. event.updateAfterEvent();

}

circle.addEventListener(MouseEvent.MOUSE_DOWN, startDragging); circle.addEventListener(MouseEvent.MOUSE_UP, stopDragging);

**square.addEventListener(MouseEvent.MOUSE_DOWN, startDragging); square.addEventListener(MouseEvent.MOUSE_UP, stopDragging);**

To extend this effect further, such as for a game where tokens or cards are moved among piles, you could add the dragged object to the Stage’s display list when it’s “picked up,” and then add it to another display list—such as the “pile” where it is dropped—when the mouse button is released.

Finally, to enhance the effect, you could apply a drop shadow filter to the display object when it is clicked (when you start dragging it) and remove the drop shadow when the object is released. For details on using the drop shadow filter and other display object filters in ActionScript, see

“Filtering display objects” on page 267

.

### Panning and scrolling display objects {#panning-and-scrolling-display-objects}

Flash Player 9 and later, Adobe AIR 1.0 and later

If you have a display object that is too large for the area in which you want it to display it, you can use the scrollRect property to define the viewable area of the display object. In addition, by changing the scrollRect property in response to user input, you can cause the content to pan left and right or scroll up and down.

The scrollRect property is an instance of the Rectangle class, which is a class that combines the values needed to define a rectangular area as a single object. To initially define the viewable area of the display object, create a new Rectangle instance and assign it to the display object’s scrollRect property. Later, to scroll or pan, you read the scrollRect property into a separate Rectangle variable, and change the desired property (for instance, change the Rectangle instance’s x property to pan or y property to scroll). Then you reassign that Rectangle instance to the scrollRect property to notify the display object of the changed value.

For example, the following code defines the viewable area for a TextField object named bigText that is too tall to fit in the SWF file’s boundaries. When the two buttons named up and down are clicked, they call functions that cause the contents of the TextField object to scroll up or down by modifying the y property of the scrollRect Rectangle instance.

import flash.events.MouseEvent; import flash.geom.Rectangle;

// Define the initial viewable area of the TextField instance:

// left: 0, top: 0, width: TextField&#039;s width, height: 350 pixels. bigText.scrollRect = new Rectangle(0, 0, bigText.width, 350);

// Cache the TextField as a bitmap to improve performance. bigText.cacheAsBitmap = true;

// called when the &quot;up&quot; button is clicked function scrollUp(event:MouseEvent):void

{

// Get access to the current scroll rectangle. var rect:Rectangle = bigText.scrollRect;

// Decrease the y value of the rectangle by 20, effectively

// shifting the rectangle down by 20 pixels. rect.y -= 20;

// Reassign the rectangle to the TextField to &quot;apply&quot; the change. bigText.scrollRect = rect;

}

// called when the &quot;down&quot; button is clicked function scrollDown(event:MouseEvent):void

{

// Get access to the current scroll rectangle. var rect:Rectangle = bigText.scrollRect;

// Increase the y value of the rectangle by 20, effectively

// shifting the rectangle up by 20 pixels. rect.y += 20;

// Reassign the rectangle to the TextField to &quot;apply&quot; the change. bigText.scrollRect = rect;

}

up.addEventListener(MouseEvent.CLICK, scrollUp); down.addEventListener(MouseEvent.CLICK, scrollDown);

As this example illustrates, when you work with the scrollRect property of a display object, it’s best to specify that Flash Player or AIR should cache the display object’s content as a bitmap, using the cacheAsBitmap property. When you do so, Flash Player and AIR don’t have to re-draw the entire contents of the display object each time it is scrolled, and can instead use the cached bitmap to render the necessary portion directly to the screen. For details, see

“Caching

display objects” on page 182

.

### Manipulating size and scaling objects {#manipulating-size-and-scaling-objects}

Flash Player 9 and later, Adobe AIR 1.0 and later

You can measure and manipulate the size of a display object in two ways, using either the dimension properties (width and height) or the scale properties (scaleX and scaleY).

Every display object has a width property and a height property, which are initially set to the size of the object in pixels. You can read the values of those properties to measure the size of the display object. You can also specify new values to change the size of the object, as follows:

// Resize a display object. square.width = 420;

square.height = 420;

// Determine the radius of a circle display object. var radius:Number = circle.width / 2;

Changing the height or width of a display object causes the object to scale, meaning its contents stretch or squeeze to fit in the new area. If the display object contains only vector shapes, those shapes will be redrawn at the new scale, with no loss in quality. Any bitmap graphic elements in the display object will be scaled rather than redrawn. So, for example, a digital photo whose width and height are increased beyond the actual dimensions of the pixel information in the image will be pixelated, making it look jagged.

When you change the width or height properties of a display object, Flash Player and AIR update the scaleX and

scaleY properties of the object as well.

**_Note:_ **_TextField objects are an exception to this scaling behavior. Text fields need to resize themselves to accommodate text wrapping and font sizes, so they reset their scaleX or scaleY values to 1 after resizing. However, if you adjust the scaleX or scaleY values of a TextField object, the width and height values change to accommodate the scaling values you provide._

These properties represent the relative size of the display object compared to its original size. The scaleX and scaleY properties use fraction (decimal) values to represent percentage. For example, if a display object’s width has been changed so that it’s half as wide as its original size, the object’s scaleX property will have the value .5, meaning 50 percent. If its height has been doubled, its scaleY property will have the value 2, meaning 200 percent.

// circle is a display object whose width and height are 150 pixels.

// At original size, scaleX and scaleY are 1 (100%). trace(circle.scaleX); // output: 1 trace(circle.scaleY); // output: 1

// When you change the width and height properties,

// Flash Player changes the scaleX and scaleY properties accordingly. circle.width = 100;

circle.height = 75;

trace(circle.scaleX); // output: 0.6622516556291391 trace(circle.scaleY); // output: 0.4966887417218543

Size changes are not proportional. In other words, if you change the height of a square but not its width, its proportions will no longer be the same, and it will be a rectangle instead of a square. If you want to make relative changes to the size of a display object, you can set the values of the scaleX and scaleY properties to resize the object, as an alternative to setting the width or height properties. For example, this code changes the width of the display object named square, and then alters the vertical scale (scaleY) to match the horizontal scale, so that the size of the square stays proportional.

// Change the width directly. square.width = 150;

// Change the vertical scale to match the horizontal scale,

// to keep the size proportional. square.scaleY = square.scaleX;

### Controlling distortion when scaling {#controlling-distortion-when-scaling}

Flash Player 9 and later, Adobe AIR 1.0 and later

Normally when a display object is scaled (for example, stretched horizontally), the resulting distortion is spread equally across the object, so that each part is stretched the same amount. For graphics and design elements, this is probably what you want. However, sometimes it’s preferable to have control over which portions of the display object stretch and which portions remain unchanged. One common example of this is a button that’s a rectangle with rounded corners. With normal scaling, the corners of the button will stretch, making the corner radius change as the button resizes.

![<Scaling distortion on a button with rounded corners>](C:\Development\Books\HaxeDevelopersGuide\export\assets\scaling_distortion_on_a_button_wit.png)

However, in this case it would be preferable to have control over the scaling—to be able to designate certain areas which should scale (the straight sides and middle) and areas which shouldn’t (the corners)—so that scaling happens without visible distortion.

![<Button scaled without distortion>](C:\Development\Books\HaxeDevelopersGuide\export\assets\button_scaled_without_distortion.png)

You can use 9-slice scaling (Scale-9) to create display objects where you have control over how the objects scale. With 9-slice scaling, the display object is divided into nine separate rectangles (a 3 by 3 grid, like the grid of a tic-tac-toe board). The rectangles aren’t necessarily the same size—you designate where the grid lines are placed. Any content that lies in the four corner rectangles (such as the rounded corners of a button) will not be stretched or compressed when the display object scales. The top-center and bottom-center rectangles will scale horizontally but not vertically, while the left-middle and right-middle rectangles will scale vertically but not horizontally. The center rectangle will scale both horizontally and vertically.

![<9-slice scaling grid>](C:\Development\Books\HaxeDevelopersGuide\export\assets\9-slice_scaling_grid.png)

Keeping this in mind, if you’re creating a display object and you want certain content to never scale, you just have to make sure that the dividing lines of the 9-slice scaling grid are placed so that the content ends up in one of the corner rectangles.

In ActionScript, setting a value for the scale9Grid property of a display object turns on 9-slice scaling for the object and defines the size of the rectangles in the object’s Scale-9 grid. You use an instance of the Rectangle class as the value for the scale9Grid property, as follows:

myButton.scale9Grid = new Rectangle(32, 27, 71, 64);

The four parameters of the Rectangle constructor are the x coordinate, y coordinate, width, and height. In this example, the rectangle’s top-left corner is placed at the point x: 32, y: 27 on the display object named myButton. The rectangle is 71 pixels wide and 64 pixels tall (so its right edge is at the x coordinate 103 on the display object and its bottom edge is at the y coordinate 92 on the display object).

![<Core rectangle of the Scale-9 grid>](C:\Development\Books\HaxeDevelopersGuide\export\assets\core_rectangle_of_the_scale-9_grid.png)

The actual area contained in the region defined by the Rectangle instance represents the center rectangle of the Scale- 9 grid. The other rectangles are calculated by Flash Player and AIR by extending the sides of the Rectangle instance, as shown here:

![<Complete Scale-9 grid derived from core rectangle>](C:\Development\Books\HaxeDevelopersGuide\export\assets\complete_scale-9_grid_derived_from.png)

In this case, as the button scales up or down, the rounded corners will not stretch or compress, but the other areas will adjust to accommodate the scaling.

*   1.  **B C**

**_A._ **_myButton.width = 131;myButton.height = 106;_ **_B._ **_myButton.width = 73;myButton.height = 69;_ **_C._ **_myButton.width = 54;myButton.height_

_= 141;_

### Caching display objects {#caching-display-objects}

Flash Player 9 and later, Adobe AIR 1.0 and later

As your designs in Flash grow in size, whether you are creating an application or complex scripted animations, you need to consider performance and optimization. When you have content that remains static (such as a rectangle Shape instance), Flash Player and AIR do not optimize the content. Therefore, when you change the position of the rectangle, Flash Player or AIR redraws the entire Shape instance.

You can cache specified display objects to improve the performance of your SWF file. The display object is a _surface_, essentially a bitmap version of the instance’s vector data, which is data that you do not intend to change much over the course of your SWF file. Therefore, instances with caching turned on are not continually redrawn as the SWF file plays, letting the SWF file render quickly.

**_Note:_ **_You can update the vector data, at which time the surface is recreated. Therefore, the vector data cached in the surface does not need to remain the same for the entire SWF file._

Setting a display object’s cacheAsBitmap property to true makes the display object cache a bitmap representation of itself. Flash Player or AIR creates a surface object for the instance, which is a cached bitmap instead of vector data. If you change the bounds of the display object, the surface is recreated instead of resized. Surfaces can nest within other surfaces. The child surface copies its bitmap onto its parent surface. For more information, see

“Enabling bitmap

caching” on page 184

.

The DisplayObject class’s opaqueBackground property and scrollRect property are related to bitmap caching using the cacheAsBitmap property. Although these three properties are independent of each other, the opaqueBackground and scrollRect properties work best when an object is cached as a bitmap—you see performance benefits for the opaqueBackground and scrollRect properties only when you set cacheAsBitmap to true. For more information about scrolling display object content, see

“Panning and scrolling display objects” on page 178

. For more information about setting an opaque background, see

“Setting an opaque background color” on page 185

.

For information on alpha channel masking, which requires you to set the cacheAsBitmap property to true, see

“Masking display objects” on page 190

.

**When to enable caching**

Flash Player 9 and later, Adobe AIR 1.0 and later

Enabling caching for a display object creates a surface, which has several advantages, such as helping complex vector animations to render fast. There are several scenarios in which you will want to enable caching. It might seem as though you would always want to enable caching to improve the performance of your SWF files; however, there are situations in which enabling caching does not improve performance, or can even decrease it. This section describes scenarios in which caching should be used, and when to use regular display objects.

Overall performance of cached data depends on how complex the vector data of your instances are, how much of the data you change, and whether or not you set the opaqueBackground property. If you are changing small regions, the difference between using a surface and using vector data could be negligible. You might want to test both scenarios with your work before you deploy the application.

When to use bitmap caching

The following are typical scenarios in which you might see significant benefits when you enable bitmap caching.

*   Complex background image: An application that contains a detailed and complex background image of vector data (perhaps an image where you applied the trace bitmap command, or artwork that you created in Adobe Illustrator®). You might animate characters over the background, which slows the animation because the background needs to continuously regenerate the vector data. To improve performance, you can set the opaqueBackground property of the background display object to true. The background is rendered as a bitmap and can be redrawn quickly, so that your animation plays much faster.
*   Scrolling text field: An application that displays a large amount of text in a scrolling text field. You can place the text field in a display object that you set as scrollable with scrolling bounds (the scrollRect property). This enables fast pixel scrolling for the specified instance. When a user scrolls the display object instance, Flash Player or AIR shifts the scrolled pixels up and generates the newly exposed region instead of regenerating the entire text field.
*   Windowing system: An application with a complex system of overlapping windows. Each window can be open or closed (for example, web browser windows). If you mark each window as a surface (by setting the cacheAsBitmap property to true), each window is isolated and cached. Users can drag the windows so that they overlap each other, and each window doesn’t need to regenerate the vector content.
*   Alpha channel masking: When you are using alpha channel masking, you must set the cacheAsBitmap property to true. For more information, see

    “Masking display objects” on page 190

    .

Enabling bitmap caching in all of these scenarios improves the responsiveness and interactivity of the application by optimizing the vector graphics.

In addition, whenever you apply a filter to a display object, cacheAsBitmap is automatically set to true, even if you explicitly set it to false. If you clear all the filters from the display object, the cacheAsBitmap property returns to the value it was last set to.

When to avoid using bitmap caching

Using this feature in the wrong circumstances can negatively affect the performance of your SWF file. When you use bitmap caching, remember the following guidelines:

*   Do not overuse surfaces (display objects with caching enabled). Each surface uses more memory than a regular display object, which means that you should only enable surfaces when you need to improve rendering performance.

A cached bitmap can use significantly more memory than a regular display object. For example, if a Sprite instance on the Stage is 250 pixels by 250 pixels in size, when cached it might use 250 KB instead of 1 KB when it’s a regular (un-cached) Sprite instance.

*   Avoid zooming into cached surfaces. If you overuse bitmap caching, a large amount of memory is consumed (see previous bullet), especially if you zoom in on the content.
*   Use surfaces for display object instances that are largely static (non-animating). You can drag or move the instance, but the contents of the instance should not animate or change a lot. (Animation or changing content are more likely with a MovieClip instance containing animation or a Video instance.) For example, if you rotate or transform an instance, the instance changes between the surface and vector data, which is difficult to process and negatively affects your SWF file.
*   If you mix surfaces with vector data, it increases the amount of processing that Flash Player and AIR (and sometimes the computer) need to do. Group surfaces together as much as possible—for example, when you create windowing applications.
*   Do not cache objects whose graphics change frequently. Every time you scale, skew, rotate the display object, change the alpha or color transform, move child display objects, or draw using the graphics property, the bitmap cache is redrawn. If this happens every frame, the runtime must draw the object into a bitmap and then copy that bitmap onto the stage—which results in extra work compared to just drawing the uncached object to the stage. The performance tradeoff of caching versus update frequency depends on the complexity and size of the display object and can only be determined by testing the specific content.

Enabling bitmap caching

Flash Player 9 and later, Adobe AIR 1.0 and later

To enable bitmap caching for a display object, you set its cacheAsBitmap property to true: mySprite.cacheAsBitmap = true;

After you set the cacheAsBitmap property to true, you might notice that the display object automatically pixel-snaps to whole coordinates. When you test the SWF file, you should notice that any animation performed on a complex vector image renders much faster.

A surface (cached bitmap) is not created, even if cacheAsBitmap is set to true, if one or more of the following occurs:

*   The bitmap is greater than 2880 pixels in height or width.
*   The bitmap fails to allocate (because of an out-of-memory error).

Cached bitmap transform matrices

**Adobe AIR 2.0 and later (mobile profile)**

In AIR applications for mobile devices, you should set the cacheAsBitmapMatrix property whenever you set the cacheAsBitmap property. Setting this property allows you to apply a wider range of transformations to the display object without triggering rerendering.

mySprite.cacheAsBitmap = true; mySprite.cacheAsBitmapMatrix = new Matrix();

When you set this matrix property, you can apply the following additional transformation to the display object without recaching the object:

*   Move or translate without pixel-snapping
*   Rotate
*   Scale
*   Skew
*   Change alpha (between 0 and 100% transparency)

These transformations are applied directly to the cached bitmap.

### Setting an opaque background color {#setting-an-opaque-background-color}

Flash Player 9 and later, Adobe AIR 1.0 and later

You can set an opaque background for a display object. For example, when your SWF has a background that contains complex vector art, you can set the opaqueBackground property to a specified color (typically the same color as the Stage). The color is specified as a number (commonly a hexadecimal color value). The background is then treated as a bitmap, which helps optimize performance.

When you set cacheAsBitmap to true, and also set the opaqueBackground property to a specified color, the opaqueBackground property allows the internal bitmap to be opaque and rendered faster. If you do not set cacheAsBitmap to true, the opaqueBackground property adds an opaque vector-square shape to the background of the display object. It does not create a bitmap automatically.

The following example shows how to set the background of a display object to optimize performance:

myShape.cacheAsBitmap = true; myShape.opaqueBackground = 0xFF0000;

In this case, the background color of the Shape named myShape is set to red (0xFF0000). Assuming the Shape instance contains a drawing of a green triangle, on a Stage with a white background, this would show up as a green triangle with red in the empty space in the Shape instance’s bounding box (the rectangle that completely encloses the Shape).

![<Effect of setting opaqueBackground color>](C:\Development\Books\HaxeDevelopersGuide\export\assets\effect_of_setting_opaquebackground.png)

Of course, this code would make more sense if it were used with a Stage with a solid red background. On another colored background, that color would be specified instead. For example, in a SWF with a white background, the opaqueBackground property would most likely be set to 0xFFFFFF, or pure white.

### Applying blending modes {#applying-blending-modes}

Flash Player 9 and later, Adobe AIR 1.0 and later

Blending modes involve combining the colors of one image (the base image) with the colors of another image (the blend image) to produce a third image—the resulting image is the one that is actually displayed on the screen. Each pixel value in an image is processed with the corresponding pixel value of the other image to produce a pixel value for that same position in the result.

Every display object has a blendMode property that can be set to one of the following blending modes. These are constants defined in the BlendMode class. Alternatively, you can use the String values (in parentheses) that are the actual values of the constants.

*   BlendMode.ADD (&quot;add&quot;): Commonly used to create an animated lightening dissolve effect between two images.
*   BlendMode.ALPHA (&quot;alpha&quot;): Commonly used to apply the transparency of the foreground on the background. (Not supported under GPU rendering.)
*   BlendMode.DARKEN (&quot;darken&quot;): Commonly used to superimpose type. (Not supported under GPU rendering.)
*   BlendMode.DIFFERENCE (&quot;difference&quot;): Commonly used to create more vibrant colors.
*   BlendMode.ERASE (&quot;erase&quot;): Commonly used to cut out (erase) part of the background using the foreground alpha. (Not supported under GPU rendering.)
*   BlendMode.HARDLIGHT (&quot;hardlight&quot;): Commonly used to create shading effects. (Not supported under GPU rendering.)
*   BlendMode.INVERT (&quot;invert&quot;): Used to invert the background.
*   BlendMode.LAYER (&quot;layer&quot;): Used to force the creation of a temporary buffer for precomposition for a particular display object. (Not supported under GPU rendering.)
*   BlendMode.LIGHTEN (&quot;lighten&quot;): Commonly used to superimpose type. (Not supported under GPU rendering.)
*   BlendMode.MULTIPLY (&quot;multiply&quot;): Commonly used to create shadows and depth effects.
*   BlendMode.NORMAL (&quot;normal&quot;): Used to specify that the pixel values of the blend image override those of the base image.
*   BlendMode.OVERLAY (&quot;overlay&quot;): Commonly used to create shading effects. (Not supported under GPU rendering.)
*   BlendMode.SCREEN (&quot;screen&quot;): Commonly used to create highlights and lens flares.
*   BlendMode.SHADER (&quot;shader&quot;): Used to specify that a Pixel Bender shader is used to create a custom blending effect. For more information about using shaders, see

    “Working with Pixel Bender shaders” on page 300

    . (Not supported under GPU rendering.)
*   BlendMode.SUBTRACT (&quot;subtract&quot;): Commonly used to create an animated darkening dissolve effect between two images.

### Adjusting DisplayObject colors {#adjusting-displayobject-colors}

Flash Player 9 and later, Adobe AIR 1.0 and later

You can use the methods of the ColorTransform class (flash.geom.ColorTransform) to adjust the color of a display object. Each display object has a transform property, which is an instance of the Transform class, and contains information about various transformations that are applied to the display object (such as rotation, changes in scale or position, and so forth). In addition to its information about geometric transformations, the Transform class also includes a colorTransform property, which is an instance of the ColorTransform class, and provides access to make color adjustments to the display object. To access the color transformation information of a display object, you can use code such as this:

var colorInfo:ColorTransform = myDisplayObject.transform.colorTransform;

Once you’ve created a ColorTransform instance, you can read its property values to find out what color transformations have already been applied, or you can set those values to make color changes to the display object. To update the display object after any changes, you must reassign the ColorTransform instance back to the transform.colorTransform property.

var colorInfo:ColorTransform = myDisplayObject.transform.colorTransform;

// Make some color transformations here.

// Commit the change. myDisplayObject.transform.colorTransform = colorInfo;

**Setting color values with code**

Flash Player 9 and later, Adobe AIR 1.0 and later

The color property of the ColorTransform class can be used to assign a specific red, green, blue (RGB) color value to the display object. The following example uses the color property to change the color of the display object named square to blue, when the user clicks a button named blueBtn:

// square is a display object on the Stage.

// blueBtn, redBtn, greenBtn, and blackBtn are buttons on the Stage.

import flash.events.MouseEvent; import flash.geom.ColorTransform;

// Get access to the ColorTransform instance associated with square. var colorInfo:ColorTransform = square.transform.colorTransform;

// This function is called when blueBtn is clicked. function makeBlue(event:MouseEvent):void

{

// Set the color of the ColorTransform object. colorInfo.color = 0x003399;

// apply the change to the display object square.transform.colorTransform = colorInfo;

}

blueBtn.addEventListener(MouseEvent.CLICK, makeBlue);

Note that when you change a display object’s color using the color property, it completely changes the color of the entire object, regardless of whether the object previously had multiple colors. For example, if there is a display object containing a green circle with black text on top, setting the color property of that object’s associated ColorTransform instance to a shade of red will make the entire object, circle and text, turn red (so the text will no longer be distinguishable from the rest of the object).

Altering color and brightness effects with code

Flash Player 9 and later, Adobe AIR 1.0 and later

Suppose you have a display object with multiple colors (for example, a digital photo) and you don’t want to completely recolor the object; you just want to adjust the color of a display object based on the existing colors. In this scenario, the ColorTransform class includes a series of multiplier and offset properties that you can use to make this type of adjustment. The multiplier properties, named redMultiplier, greenMultiplier, blueMultiplier, and alphaMultiplier, work like colored photographic filters (or colored sunglasses), amplifying or diminishing certain colors in the display object. The offset properties (redOffset, greenOffset, blueOffset, and alphaOffset) can be used to add extra amounts of a certain color to the object, or to specify the minimum value that a particular color can have.

These multiplier and offset properties are identical to the advanced color settings that are available for movie clip symbols in the Flash authoring tool when you choose Advanced from the Color pop-up menu on the Property inspector.

The following code loads a JPEG image and applies a color transformation to it, which adjusts the red and green channels as the mouse pointer moves along the x axis and y axis. In this case, because no offset values are specified, the color value of each color channel displayed on screen will be a percentage of the original color value in the image— meaning that the most red or green displayed in any given pixel will be the original amount of red or green in that pixel.

import flash.display.Loader; import flash.events.MouseEvent; import flash.geom.Transform; import flash.geom.ColorTransform; import flash.net.URLRequest;

// Load an image onto the Stage. var loader:Loader = new Loader();

var url:URLRequest = new [URLRequest(&quot;http://www.helpexamples.com/flash/images/image1.jpg&quot;);](http://www.helpexamples.com/flash/images/image1.jpg) loader.load(url);

this.addChild(loader);

// This function is called when the mouse moves over the loaded image. function adjustColor(event:MouseEvent):void

{

// Access the ColorTransform object for the Loader (containing the image) var colorTransformer:ColorTransform = loader.transform.colorTransform;

// Set the red and green multipliers according to the mouse position.

// The red value ranges from 0% (no red) when the cursor is at the left

// to 100% red (normal image appearance) when the cursor is at the right.

// The same applies to the green channel, except it&#039;s controlled by the

// position of the mouse in the y axis. colorTransformer.redMultiplier = (loader.mouseX / loader.width) * 1;

colorTransformer.greenMultiplier = (loader.mouseY / loader.height) * 1;

// Apply the changes to the display object. loader.transform.colorTransform = colorTransformer;

}

loader.addEventListener(MouseEvent.MOUSE_MOVE, adjustColor);

### Rotating objects {#rotating-objects}

Flash Player 9 and later, Adobe AIR 1.0 and later

Display objects can be rotated using the rotation property. You can read this value to find out whether an object has been rotated, or to rotate the object you can set this property to a number (in degrees) representing the amount of rotation to be applied to the object. For instance, this line of code rotates the object named square 45 degrees (one eighth of one complete revolution):

square.rotation = 45;

Alternatively, you can rotate a display object using a transformation matrix, described in

“Working with geometry” on

page 210

.

### Fading objects {#fading-objects}

Flash Player 9 and later, Adobe AIR 1.0 and later

You can control the transparency of a display object to make it partially (or completely transparent), or change the transparency to make the object appear to fade in or out. The DisplayObject class’s alpha property defines the transparency (or more accurately, the opacity) of a display object. The alpha property can be set to any value between 0 and 1, where 0 is completely transparent, and 1 is completely opaque. For example, these lines of code make the object named myBall partially (50 percent) transparent when it is clicked with the mouse:

function fadeBall(event:MouseEvent):void

{

myBall.alpha = .5;

}

myBall.addEventListener(MouseEvent.CLICK, fadeBall);

You can also alter the transparency of a display object using the color adjustments available through the ColorTransform class. For more information, see

“Adjusting DisplayObject colors” on page 187

.

### Masking display objects {#masking-display-objects}

Flash Player 9 and later, Adobe AIR 1.0 and later

You can use a display object as a mask to create a hole through which the contents of another display object are visible.

**Defining a mask**

To indicate that a display object will be the mask for another display object, set the mask object as the mask property of the display object to be masked:

// Make the object maskSprite be a mask for the object mySprite. mySprite.mask = maskSprite;

The masked display object is revealed under all opaque (nontransparent) areas of the display object acting as the mask. For instance, the following code creates a Shape instance containing a red 100 by 100 pixel square and a Sprite instance containing a blue circle with a radius of 25 pixels. When the circle is clicked, it is set as the mask for the square, so that the only part of the square that shows is the part that is covered by the solid part of the circle. In other words, only a red circle will be visible.

// This code assumes it&#039;s being run within a display object container

// such as a MovieClip or Sprite instance. import flash.display.Shape;

// Draw a square and add it to the display list. var square:Shape = new Shape(); square.graphics.lineStyle(1, 0x000000); square.graphics.beginFill(0xff0000); square.graphics.drawRect(0, 0, 100, 100); square.graphics.endFill(); this.addChild(square);

// Draw a circle and add it to the display list. var circle:Sprite = new Sprite(); circle.graphics.lineStyle(1, 0x000000); circle.graphics.beginFill(0x0000ff); circle.graphics.drawCircle(25, 25, 25); circle.graphics.endFill(); this.addChild(circle);

function maskSquare(event:MouseEvent):void

{

square.mask = circle; circle.removeEventListener(MouseEvent.CLICK, maskSquare);

}

circle.addEventListener(MouseEvent.CLICK, maskSquare);

The display object that is acting as a mask can be draggable, animated, resized dynamically, and can use separate shapes within a single mask. The mask display object doesn’t necessarily need to be added to the display list. However, if you want the mask object to scale when the Stage is scaled or if you want to enable user interaction with the mask (such as user-controlled dragging and resizing), the mask object must be added to the display list. The actual z-index (front-to- back order) of the display objects doesn’t matter, as long as the mask object is added to the display list. (The mask object will not appear on the screen except as a mask.) If the mask object is a MovieClip instance with multiple frames, it plays all the frames in its timeline, the same as it would if it were not serving as a mask. You can remove a mask by setting the mask property to null:

// remove the mask from mySprite mySprite.mask = null;

You cannot use a mask to mask another mask. You cannot set the alpha property of a mask display object. Only fills are used in a display object that is used as a mask; strokes are ignored.

AIR 2

If a masked display object is cached by setting the cacheAsBitmap and cacheAsBitmapMatrix properties, the mask must be a child of the masked display object. Similarly, if the masked display object is a descendent of a display object container that is cached, both the mask and the display object must be descendents of that container. If the masked object is a descendent of more than one cached display object container, the mask must be a descendent of the cached container closest to the masked object in the display list.

About masking device fonts

You can use a display object to mask text that is set in a device font. When you use a display object to mask text set in a device font, the rectangular bounding box of the mask is used as the masking shape. That is, if you create a non- rectangular display object mask for device font text, the mask that appears in the SWF file is the shape of the rectangular bounding box of the mask, not the shape of the mask itself.

Alpha channel masking

Alpha channel masking is supported if both the mask and the masked display objects use bitmap caching, as shown here:

// maskShape is a Shape instance which includes a gradient fill. mySprite.cacheAsBitmap = true;

maskShape.cacheAsBitmap = true; mySprite.mask = maskShape;

For instance, one application of alpha channel masking is to use a filter on the mask object independently of a filter that is applied to the masked display object.

In the following example, an external image file is loaded onto the Stage. That image (or more accurately, the Loader instance it is loaded into) will be the display object that is masked. A gradient oval (solid black center fading to transparent at the edges) is drawn over the image; this will be the alpha mask. Both display objects have bitmap caching turned on. The oval is set as a mask for the image, and it is then made draggable.

// This code assumes it&#039;s being run within a display object container

// such as a MovieClip or Sprite instance.

import flash.display.GradientType; import flash.display.Loader; import flash.display.Sprite; import flash.geom.Matrix;

import flash.net.URLRequest;

// Load an image and add it to the display list. var loader:Loader = new Loader();

var url:URLRequest = new [URLRequest(&quot;http://www.helpexamples.com/flash/images/image1.jpg&quot;);](http://www.helpexamples.com/flash/images/image1.jpg) loader.load(url);

this.addChild(loader);

// Create a Sprite.

var oval:Sprite = new Sprite();

// Draw a gradient oval.

var colors:Array = [0x000000, 0x000000]; var alphas:Array = [1, 0];

var ratios:Array = [0, 255];

var matrix:Matrix = new Matrix(); matrix.createGradientBox(200, 100, 0, -100, -50); oval.graphics.beginGradientFill(GradientType.RADIAL,

colors, alphas, ratios, matrix);

oval.graphics.drawEllipse(-100, -50, 200, 100); oval.graphics.endFill();

// add the Sprite to the display list this.addChild(oval);

// Set cacheAsBitmap = true for both display objects. loader.cacheAsBitmap = true;

oval.cacheAsBitmap = true;

// Set the oval as the mask for the loader (and its child, the loaded image) loader.mask = oval;

// Make the oval draggable. oval.startDrag(true);