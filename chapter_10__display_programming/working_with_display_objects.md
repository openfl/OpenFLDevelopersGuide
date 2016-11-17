## Working with display objects {#working-with-display-objects}

Flash Player 9 and later, Adobe AIR 1.0 and later

Now that you understand the basic concepts of the Stage, display objects, display object containers, and the display list, this section provides you with some more specific information about working with display objects in ActionScript 3.0.

**Properties and methods of the DisplayObject class**

Flash Player 9 and later, Adobe AIR 1.0 and later

All display objects are subclasses of the DisplayObject class, and as such they inherit the properties and methods of the DisplayObject class. The properties inherited are basic properties that apply to all display objects. For example, each display object has an x property and a y property that specifies the object’s position in its display object container.

You cannot create a DisplayObject instance using the DisplayObject class constructor. You must create another type of object (an object that is a subclass of the DisplayObject class), such as a Sprite, to instantiate an object with the new operator. Also, if you want to create a custom display object class, you must create a subclass of one of the display object subclasses that has a usable constructor function (such as the Shape class or the Sprite class). For more information, see the [DisplayObject](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/DisplayObject.html) class description in the [ActionScript 3.0 Reference for the Adobe Flash Platform](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/DisplayObject.html).

### Adding display objects to the display list {#adding-display-objects-to-the-display-list}

Flash Player 9 and later, Adobe AIR 1.0 and later

When you instantiate a display object, it will not appear on-screen (on the Stage) until you add the display object instance to a display object container that is on the display list. For example, in the following code, the myText TextField object would not be visible if you omitted the last line of code. In the last line of code, the this keyword must refer to a display object container that is already added to the display list.

import flash.display.*; import flash.text.TextField;

var myText:TextField = new TextField(); myText.text = &quot;Buenos dias.&quot;; this.addChild(myText);

When you add any visual element to the Stage, that element becomes a _child_ of the Stage object. The first SWF file loaded in an application (for example, the one that you embed in an HTML page) is automatically added as a child of the Stage. It can be an object of any type that extends the Sprite class.

Any display objects that you create _without_ using ActionScript—for example, by adding an MXML tag in a Flex MXML file or by placing an item on the Stage in Flash Professional—are added to the display list. Although you do not add these display objects through ActionScript, you can access them through ActionScript. For example, the following code adjusts the width of an object named button1 that was added in the authoring tool (not through ActionScript):

button1.width = 200;

### Working with display object containers {#working-with-display-object-containers}

Flash Player 9 and later, Adobe AIR 1.0 and later

If a DisplayObjectContainer object is deleted from the display list, or if it is moved or transformed in some other way, each display object in the DisplayObjectContainer is also deleted, moved, or transformed.

A display object container is itself a type of display object—it can be added to another display object container. For example, the following image shows a display object container, pictureScreen, that contains one outline shape and four other display object containers (of type PictureFrame):

**A B**

1.  _A shape defining the border of the pictureScreen display object container_ **_B._ **_Four display object containers that are children of the pictureScreen object_

In order to have a display object appear in the display list, you must add it to a display object container that is on the display list. You do this by using the addChild() method or the addChildAt() method of the container object. For example, without the final line of the following code, the myTextField object would not be displayed:

var myTextField:TextField = new TextField(); myTextField.text = &quot;hello&quot;; this.root.addChild(myTextField);

In this code sample, this.root points to the MovieClip display object container that contains the code. In your actual code, you may specify a different container.

Use the addChildAt() method to add the child to a specific position in the child list of the display object container. These zero-based index positions in the child list relate to the layering (the front-to-back order) of the display objects. For example, consider the following three display objects. Each object was created from a custom class called Ball.

The layering of these display objects in their container can be adjusted using the addChildAt() method. For example, consider the following code:

ball_A = new Ball(0xFFCC00, &quot;a&quot;); ball_A.name = &quot;ball_A&quot;;

ball_A.x = 20;

ball_A.y = 20; container.addChild(ball_A);

ball_B = new Ball(0xFFCC00, &quot;b&quot;); ball_B.name = &quot;ball_B&quot;;

ball_B.x = 70;

ball_B.y = 20; container.addChild(ball_B);

ball_C = new Ball(0xFFCC00, &quot;c&quot;); ball_C.name = &quot;ball_C&quot;;

ball_C.x = 40;

ball_C.y = 60;

container.addChildAt(ball_C, 1);

After executing this code, the display objects are positioned as follows in the container DisplayObjectContainer object. Notice the layering of the objects.

To reposition an object to the top of the display list, simply re-add it to the list. For example, after the previous code, to move ball_A to the top of the stack, use this line of code:

container.addChild(ball_A);

This code effectively removes ball_A from its location in container’s display list, and re-adds it to the top of the list— which has the end result of moving it to the top of the stack.

You can use the getChildAt() method to verify the layer order of the display objects. The getChildAt() method returns child objects of a container based on the index number you pass it. For example, the following code reveals names of display objects at different positions in the child list of the container DisplayObjectContainer object:

trace(container.getChildAt(0).name); // ball_A trace(container.getChildAt(1).name); // ball_C trace(container.getChildAt(2).name); // ball_B

If you remove a display object from the parent container’s child list, the higher elements on the list each move down a position in the child index. For example, continuing with the previous code, the following code shows how the display object that was at position 2 in the container DisplayObjectContainer moves to position 1 if a display object that is lower in the child list is removed:

container.removeChild(ball_C); trace(container.getChildAt(0).name); // ball_A trace(container.getChildAt(1).name); // ball_B

The removeChild() and removeChildAt() methods do not delete a display object instance entirely. They simply remove it from the child list of the container. The instance can still be referenced by another variable. (Use the delete operator to completely remove an object.)

Because a display object has only one parent container, you can add an instance of a display object to only one display object container. For example, the following code shows that the display object tf1 can exist in only one container (in this case, a Sprite, which extends the DisplayObjectContainer class):

tf1:TextField = new TextField(); tf2:TextField = new TextField(); tf1.name = &quot;text 1&quot;;

tf2.name = &quot;text 2&quot;;

container1:Sprite = new Sprite(); container2:Sprite = new Sprite();

container1.addChild(tf1); container1.addChild(tf2); container2.addChild(tf1);

trace(container1.numChildren); // 1 trace(container1.getChildAt(0).name); // text 2 trace(container2.numChildren); // 1 trace(container2.getChildAt(0).name); // text 1

If you add a display object that is contained in one display object container to another display object container, it is removed from the first display object container’s child list.

In addition to the methods described above, the DisplayObjectContainer class defines several methods for working with child display objects, including the following:

*   contains(): Determines whether a display object is a child of a DisplayObjectContainer.
*   getChildByName(): Retrieves a display object by name.
*   getChildIndex(): Returns the index position of a display object.
*   setChildIndex(): Changes the position of a child display object.
*   removeChildren(): Removes multiple child display objects.
*   swapChildren(): Swaps the front-to-back order of two display objects.
*   swapChildrenAt(): Swaps the front-to-back order of two display objects, specified by their index values. For more information, see the relevant entries in the [ActionScript 3.0 Reference for the Adobe Flash Platform](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/DisplayObjectContainer.html)[.](http://www.adobe.com/go/learn_flashcs4_langref_en)

Recall that a display object that is off the display list—one that is not included in a display object container that is a child of the Stage—is known as an _off-list_ display object.

### Traversing the display list {#traversing-the-display-list}

Flash Player 9 and later, Adobe AIR 1.0 and later

As you’ve seen, the display list is a tree structure. At the top of the tree is the Stage, which can contain multiple display objects. Those display objects that are themselves display object containers can contain other display objects, or display object containers.

The DisplayObjectContainer class includes properties and methods for traversing the display list, by means of the child lists of display object containers. For example, consider the following code, which adds two display objects, title and pict, to the container object (which is a Sprite, and the Sprite class extends the DisplayObjectContainer class):

var container:Sprite = new Sprite(); var title:TextField = new TextField(); title.text = &quot;Hello&quot;;

var pict:Loader = new Loader();

var url:URLRequest = new URLRequest(&quot;banana.jpg&quot;); pict.load(url);

pict.name = &quot;banana loader&quot;; container.addChild(title); container.addChild(pict);

The getChildAt() method returns the child of the display list at a specific index position:

trace(container.getChildAt(0) is TextField); // true

You can also access child objects by name. Each display object has a name property, and if you don’t assign it, Flash Player or AIR assigns a default value, such as &quot;instance1&quot;. For example, the following code shows how to use the getChildByName() method to access a child display object with the name &quot;banana loader&quot;:

trace(container.getChildByName(&quot;banana loader&quot;) is Loader); // true

Using the getChildByName() method can result in slower performance than using the getChildAt() method.

Since a display object container can contain other display object containers as child objects in its display list, you can traverse the full display list of the application as a tree. For example, in the code excerpt shown earlier, once the load operation for the pict Loader object is complete, the pict object will have one child display object, which is the bitmap, loaded. To access this bitmap display object, you can write pict.getChildAt(0). You can also write container.getChildAt(0).getChildAt(0) (since container.getChildAt(0) == pict).

The following function provides an indented trace() output of the display list from a display object container:

function traceDisplayList(container:DisplayObjectContainer,indentString:String = &quot;&quot;):void

{

var child:DisplayObject;

for (var i:uint=0; i &lt; container.numChildren; i++)

{

child = container.getChildAt(i); trace(indentString, child, child.name);

if (container.getChildAt(i) is DisplayObjectContainer)

{

traceDisplayList(DisplayObjectContainer(child), indentString + &quot;&quot;)

}

}

}

Adobe Flex

If you use Flex, you should know that Flex defines many component display object classes, and these classes override the display list access methods of the DisplayObjectContainer class. For example, the Container class of the mx.core package overrides the addChild() method and other methods of the DisplayObjectContainer class (which the Container class extends). In the case of the addChild() method, the class overrides the method in such a way that you cannot add all types of display objects to a Container instance in Flex. The overridden method, in this case, requires that the child object that you are adding be a type of mx.core.UIComponent object.

### Setting Stage properties {#setting-stage-properties}

Flash Player 9 and later, Adobe AIR 1.0 and later

The Stage class overrides most properties and methods of the DisplayObject class. If you call one of these overridden properties or methods, Flash Player and AIR throw an exception. For example, the Stage object does not have x or y properties, since its position is fixed as the main container for the application. The x and y properties refer to the position of a display object relative to its container, and since the Stage is not contained in another display object container, these properties do not apply.

**_Note:_ **_Some properties and methods of the Stage class are only available to display objects that are in the same security sandbox as the first SWF file loaded. For details, see_

_“Stage security” on page 1063_

_._

**Controlling the playback frame rate**

Flash Player 9 and later, Adobe AIR 1.0 and later

The frameRate property of the Stage class is used to set the frame rate for all SWF files loaded into the application. For more information, see the [ActionScript 3.0 Reference for the Adobe Flash Platform](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Stage.html#frameRate).

Controlling Stage scaling

Flash Player 9 and later, Adobe AIR 1.0 and later

When the portion of the screen representing Flash Player or AIR is resized, the runtime automatically adjusts the Stage contents to compensate. The Stage class’s scaleMode property determines how the Stage contents are adjusted. This property can be set to four different values, defined as constants in the flash.display.StageScaleMode class:

*   StageScaleMode.EXACT_FIT scales the SWF to fill the new stage dimensions without regard for the original content aspect ratio. The scale factors might not be the same for width and height, so the content can appear squeezed or stretched if the aspect ratio of the stage is changed.
*   StageScaleMode.SHOW_ALL scales the SWF to fit entirely within the new stage dimensions without changing the content aspect ratio. This scale mode displays all of the content, but can result in “letterbox” borders, like the black bars that appear when viewing a wide-screen movie on a standard television.
*   StageScaleMode.NO_BORDER scales the SWF to entirely fill the new stage dimensions without changing the aspect ratio of the content. This scale mode makes full use of the stage display area, but can result in cropping.
*   StageScaleMode.NO_SCALE — does not scale the SWF. If the new stage dimensions are smaller, the content is cropped; if larger, the added space is blank.

In the StageScaleMode.NO_SCALE scale mode only, the stageWidth and stageHeight properties of the Stage class can be used to determine the actual pixel dimensions of the resized stage. (In the other scale modes, the stageWidth and stageHeight properties always reflect the original width and height of the SWF.) In addition, when scaleMode is set to StageScaleMode.NO_SCALE and the SWF file is resized, the Stage class’s resize event is dispatched, allowing you to make adjustments accordingly.

Consequently, having scaleMode set to StageScaleMode.NO_SCALE allows you to have greater control over how the screen contents adjust to the window resizing if you desire. For example, in a SWF containing a video and a control bar, you might want to make the control bar stay the same size when the Stage is resized, and only change the size of the video window to accommodate the Stage size change. This is demonstrated in the following example:

// mainContent is a display object containing the main content;

// it is positioned at the top-left corner of the Stage, and

// it should resize when the SWF resizes.

// controlBar is a display object (e.g. a Sprite) containing several

// buttons; it should stay positioned at the bottom-left corner of the

// Stage (below mainContent) and it should not resize when the SWF

// resizes.

import flash.display.Stage; import flash.display.StageAlign;

import flash.display.StageScaleMode; import flash.events.Event;

var swfStage:Stage = mainContent.stage; swfStage.scaleMode = StageScaleMode.NO_SCALE; swfStage.align = StageAlign.TOP_LEFT; swfStage.addEventListener(Event.RESIZE, resizeDisplay);

function resizeDisplay(event:Event):void

{

var swfWidth:int = swfStage.stageWidth; var swfHeight:int = swfStage.stageHeight;

// Resize the main content area

var newContentHeight:Number = swfHeight - controlBar.height; mainContent.height = newContentHeight;

mainContent.scaleX = mainContent.scaleY;

// Reposition the control bar. controlBar.y = newContentHeight;

}

Setting the stage scale mode for AIR windows

The stage scaleMode property determines how the stage scales and clips child display objects when a window is resized. Only the noScale mode should be used in AIR. In this mode, the stage is not scaled. Instead, the size of the stage changes directly with the bounds of the window. Objects may be clipped if the window is resized smaller.

The stage scale modes are designed for use in a environments such as a web browser where you don&#039;t always have control over the size or aspect ratio of the stage. The modes let you choose the least bad compromise when the stage does not match the ideal size or aspect ratio of your application. In AIR, you always have control of the stage, so in most cases re-laying out your content or adjusting the dimensions of your window will give you better results than enabling stage scaling.

In the browser and for the initial AIR window, the relationship between the window size and the initial scale factor is read from the loaded SWF file. However, when you create a NativeWindow object, AIR chooses an arbitrary relationship between the window size and the scale factor of 72:1\. Thus, if your window is 72x72 pixels, a 10x10 rectangle added to the window is drawn the correct size of 10x10 pixels. However, if the window is 144x144 pixels, then a 10x10 pixel rectangle is scaled to 20x20 pixels. If you insist on using a scaleMode other than noScale for a window stage, you can compensate by setting the scale factor of any display objects in the window to the ratio of 72 pixels to the current width and height of the stage. For example, the following code calculates the required scale factor for a display object named client:

if(newWindow.stage.scaleMode != StageScaleMode.NO_SCALE){ client.scaleX = 72/newWindow.stage.stageWidth; client.scaleY = 72/newWindow.stage.stageHeight;

}

**_Note:_ **_Flex and HTML windows automatically set the stage scaleMode to noScale. Changing the scaleMode disturbs the automatic layout mechanisms used in these types of windows._

Working with full-screen mode

Flash Player 9 and later, Adobe AIR 1.0 and later

Full-screen mode allows you to set a movie’s stage to fill a viewer’s entire monitor without any container borders or menus. The Stage class’s displayState property is used to toggle full-screen mode on and off for a SWF. The displayState property can be set to one of the values defined by the constants in the flash.display.StageDisplayState class. To turn on full-screen mode, set the displayState property to StageDisplayState.FULL_SCREEN:

stage.displayState = StageDisplayState.FULL_SCREEN;

To turn on full-screen interactive mode (new in Flash Player 11.3), set the displayState property to

StageDisplayState.FULL_SCREEN_INTERACTIVE:

stage.displayState = StageDisplayState.FULL_SCREEN_INTERACTIVE;

In Flash Player, full-screen mode can only be initiated through ActionScript in response to a mouse click (including right-click) or keypress. AIR content running in the application security sandbox does not require that full-screen mode be entered in response to a user gesture.

To exit full-screen mode, set the displayState property to StageDisplayState.NORMAL. stage.displayState = StageDisplayState.NORMAL;

In addition, a user can choose to leave full-screen mode by switching focus to a different window or by using one of several key combinations: the Esc key (all platforms), Control-W (Windows), Command-W (Mac), or Alt-F4 (Windows).

Enabling full-screen mode in Flash Player

To enable full-screen mode for a SWF file embedded in an HTML page, the HTML code to embed Flash Player must include a param tag and embed attribute with the name allowFullScreen and value true, like this:

&lt;object&gt;

...

&lt;param name=&quot;allowFullScreen&quot; value=&quot;true&quot; /&gt;

&lt;embed ... allowFullScreen=&quot;true&quot; /&gt;

&lt;/object&gt;

In the Flash authoring tool, select File -&gt; Publish Settings and in the Publish Settings dialog box, on the HTML tab, select the Flash Only - Allow Full Screen template.

In Flex, ensure that the HTML template includes &lt;object&gt; and &lt;embed&gt; tags that support full screen.

If you are using JavaScript in a web page to generate the SWF-embedding tags, you must alter the JavaScript to add the allowFullScreen param tag and attribute. For example, if your HTML page uses the AC_FL_RunContent() function (which is used in HTML pages generated by Flash Professional and Flash Builder), you should add the allowFullScreen parameter to that function call as follows:

AC_FL_RunContent(

...

&#039;allowFullScreen&#039;,&#039;true&#039;,

...

); //end AC code

This does not apply to SWF files running in the stand-alone Flash Player.

**_Note:_ **_If you set the Window Mode (wmode in the HTML) to Opaque Windowless (opaque) or Transparent Windowless (transparent), the full-screen window is always opaque_

There are also security-related restrictions for using full-screen mode with Flash Player in a browser. These restrictions are described in

“Security” on page 1042

.

Enabling full-screen interactive mode in Flash Player 11.3 and higher

Flash Player 11.3 and higher support full-screen interactive mode, which enables full support for all keyboard keys (except for **Esc**, which exits full-screen interactive mode). Full-screen interactive mode is useful for gaming (for example, to enable chat in a multi-player game or WASD keyboard controls in a first-person shooter game.)

To enable full-screen interactive mode for a SWF file embedded in an HTML page, the HTML code to embed Flash Player must include a param tag and embed attribute with the name allowFullScreenInteractive and value true, like this:

&lt;object&gt;

...

&lt;param name=&quot;allowFullScreenInteractive&quot; value=&quot;true&quot; /&gt;

&lt;embed ... allowFullScreenInteractive=&quot;true&quot; /&gt;

&lt;/object&gt;

In the Flash authoring tool, select File -&gt; Publish Settings and in the Publish Settings dialog box, on the HTML tab, select the Flash Only - Allow Full Screen Interactive template.

In Flash Builder and Flex, ensure that the HTML templates include &lt;object&gt; and &lt;embed&gt; tags that support full screen interactive mode.

If you are using JavaScript in a web page to generate the SWF-embedding tags, you must alter the JavaScript to add the allowFullScreenInteractive param tag and attribute. For example, if your HTML page uses the AC_FL_RunContent() function (which is used in HTML pages generated by Flash Professional and Flash Builder), you should add the allowFullScreenInteractive parameter to that function call as follows:

AC_FL_RunContent(

...

&#039;allowFullScreenInteractive&#039;,&#039;true&#039;,

...

); //end AC code

This does not apply to SWF files running in the stand-alone Flash Player.

Full screen stage size and scaling

The Stage.fullScreenHeight and Stage.fullScreenWidth properties return the height and the width of the monitor that’s used when going to full-screen size, if that state is entered immediately. These values can be incorrect if the user has the opportunity to move the browser from one monitor to another after you retrieve these values but before entering full-screen mode. If you retrieve these values in the same event handler where you set the Stage.displayState property to StageDisplayState.FULL_SCREEN, the values are correct.For users with multiple

monitors, the SWF content expands to fill only one monitor. Flash Player and AIR use a metric to determine which monitor contains the greatest portion of the SWF, and uses that monitor for full-screen mode. The fullScreenHeight and fullScreenWidth properties only reflect the size of the monitor that is used for full-screen mode. For more information, see [Stage.fullScreenHeight](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Stage.html#fullScreenHeight) and [Stage.fullScreenWidth](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Stage.html#fullScreenWidth) in the [ActionScript 3.0 Reference for the](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Stage.html#fullScreenHeight) [Adobe Flash Platform](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Stage.html#fullScreenHeight).

Stage scaling behavior for full-screen mode is the same as under normal mode; the scaling is controlled by the Stage class’s scaleMode property. If the scaleMode property is set to StageScaleMode.NO_SCALE, the Stage’s stageWidth and stageHeight properties change to reflect the size of the screen area occupied by the SWF (the entire screen, in this case); if viewed in the browser the HTML parameter for this controls the setting.

You can use the Stage class’s fullScreen event to detect and respond when full-screen mode is turned on or off. For example, you might want to reposition, add, or remove items from the screen when entering or leaving full-screen mode, as in this example:

import flash.events.FullScreenEvent;

function fullScreenRedraw(event:FullScreenEvent):void

{

if (event.fullScreen)

{

// Remove input text fields.

// Add a button that closes full-screen mode.

}

else

{

// Re-add input text fields.

// Remove the button that closes full-screen mode.

}

}

mySprite.stage.addEventListener(FullScreenEvent.FULL_SCREEN, fullScreenRedraw);

As this code shows, the event object for the fullScreen event is an instance of the flash.events.FullScreenEvent class, which includes a fullScreen property indicating whether full-screen mode is enabled (true) or not (false).

Keyboard support in full-screen mode

When Flash Player runs in a browser, all keyboard-related ActionScript, such as keyboard events and text entry in TextField instances, is disabled in full-screen mode. The exceptions (the keys that are enabled) are:

*   Selected non-printing keys, specifically the arrow keys, space bar, and tab key
*   Keyboard shortcuts that terminate full-screen mode: Esc (Windows and Mac), Control-W (Windows), Command- W (Mac), and Alt-F4

These restrictions are not present for SWF content running in the stand-alone Flash Player or in AIR. AIR supports an interactive full-screen mode that allows keyboard input.

Mouse support in full-screen mode

By default, mouse events in full-screen mode work the same way as when not in full-screen mode. However, in full- screen mode, you can optionally set the Stage.mouseLock property to enable mouse locking. Mouse locking disables the cursor and enables unbounded mouse movement.

**_Note:_ **_You can only enable mouse locking in full-screen mode for desktop applications. Setting it on applications not in full-screen mode, or for applications on mobile devices, throws an exception._

Mouse locking is disabled automatically and the mouse cursor is made visible again when:

*   The user exits full-screen mode by using the Escape key (all platforms), Control-W (Windows), Command-W (Mac), or Alt-F4 (Windows).
*   The application window loses focus.
*   Any settings UI is visible, including all privacy dialog boxes.
*   A native dialog box is shown, such as a file upload dialog box.

Events associated with mouse movement, such as the mouseMove event, use the MouseEvent class to represent the event object. When mouse locking is disabled, use the MouseEvent.localX and MouseEvent.localY properties to determine the location of the mouse.When mouse locking is enabled, use the MouseEvent.movementX and MouseEvent.movementY properties to determine the location of the mouse. The movementX and movementY properties contain changes in the position of the mouse since the last event, instead of absolute coordinates of the mouse location.

Hardware scaling in full-screen mode

You can use the Stage class’s fullScreenSourceRect property to set Flash Player or AIR to scale a specific region of the stage to full-screen mode. Flash Player and AIR scale in hardware, if available, using the graphics and video card on a user&#039;s computer, and generally display content more quickly than software scaling.

To take advantage of hardware scaling, you set the whole stage or part of the stage to full-screen mode. The following ActionScript 3.0 code sets the whole stage to full-screen mode:

import flash.geom.*;

{

stage.fullScreenSourceRect = new Rectangle(0,0,320,240); stage.displayState = StageDisplayState.FULL_SCREEN;

}

When this property is set to a valid rectangle and the displayState property is set to full-screen mode, Flash Player and AIR scale the specified area. The actual Stage size in pixels within ActionScript does not change. Flash Player and AIR enforce a minimum limit for the size of the rectangle to accommodate the standard “Press Esc to exit full-screen mode” message. This limit is usually around 260 by 30 pixels but can vary depending on platform and Flash Player version.

_The fullScreenSourceRect property can only be set when Flash Player or AIR is not in full-screen mode. To use this property correctly, set this property first, then set the displayState property to full-screen mode._

To enable scaling, set the fullScreenSourceRect property to a rectangle object.

stage.fullScreenSourceRect = new Rectangle(0,0,320,240); To disable scaling, set the fullScreenSourceRect property to null. stage.fullScreenSourceRect = null;

To take advantage of all hardware acceleration features with Flash Player, enable it through the Flash Player Settings dialog box. To load the dialog box, right-click (Windows) or Control-click (Mac) inside Flash Player content in your browser. Select the Display tab, which is the first tab, and click the checkbox: Enable hardware acceleration.

Direct and GPU-compositing window modes

Flash Player 10 introduces two window modes, direct and GPU compositing, which you can enable through the publish settings in the Flash authoring tool. These modes are not supported in AIR. To take advantage of these modes, you must enable hardware acceleration for Flash Player.

Direct mode uses the fastest, most direct path to push graphics to the screen, which is advantageous for video playback.

GPU Compositing uses the graphics processing unit on the video card to accelerate compositing. Video compositing is the process of layering multiple images to create a single video image. When compositing is accelerated with the GPU it can improve the performance of YUV conversion, color correction, rotation or scaling, and blending. YUV conversion refers to the color conversion of composite analog signals, which are used for transmission, to the RGB (red, green, blue) color model that video cameras and displays use. Using the GPU to accelerate compositing reduces the memory and computational demands that are otherwise placed on the CPU. It also results in smoother playback for standard-definition video.

Be cautious in implementing these window modes. Using GPU compositing can be expensive for memory and CPU resources. If some operations (such as blend modes, filtering, clipping or masking) cannot be carried out in the GPU, they are done by the software. Adobe recommends limiting yourself to one SWF file per HTML page when using these modes and you should not enable these modes for banners. The Flash Test Movie facility does not use hardware acceleration but you can use it through the Publish Preview option.

Setting a frame rate in your SWF file that is higher than 60, the maximum screen refresh rate, is useless. Setting the frame rate from 50 through 55 allows for dropped frames, which can occur for various reasons from time to time.

Using direct mode requires Microsoft DirectX 9 with VRAM 128 MB on Windows and OpenGL for Apple Macintosh, Mac OS X v10.2 or higher. GPU compositing requires Microsoft DirectX 9 and Pixel Shader 2.0 support on Windows with 128 MB of VRAM. On Mac OS X and Linux, GPU compositing requires OpenGL 1.5 and several OpenGL extensions (framebuffer object, multitexture, shader objects, shading language, fragment shader).

You can activate direct and gpu acceleration modes on a per-SWF basis through the Flash Publish Settings dialog box, using the Hardware Acceleration menu on the Flash tab. If you choose None, the window mode reverts to default, transparent, or opaque, as specified by the Window Mode setting on the HTML tab.

### Handling events for display objects {#handling-events-for-display-objects}

Flash Player 9 and later, Adobe AIR 1.0 and later

The DisplayObject class inherits from the EventDispatcher class. This means that every display object can participate fully in the event model (described in

“Handling events” on page 125

). Every display object can use its addEventListener() method—inherited from the EventDispatcher class—to listen for a particular event, but only if the listening object is part of the event flow for that event.

When Flash Player or AIR dispatches an event object, that event object makes a round-trip journey from the Stage to the display object where the event occurred. For example, if a user clicks on a display object named child1, Flash Player dispatches an event object from the Stage through the display list hierarchy down to the child1 display object.

The event flow is conceptually divided into three phases, as illustrated in this diagram:

**Stage**

**Capture Phase**

**Bubbling Phase**

**Parent Node**

**Child1 Node Child2 Node**

**Target Phase**

For more information, see

“Handling events” on page 125

.

One important issue to keep in mind when working with display object events is the effect that event listeners can have on whether display objects are automatically removed from memory (garbage collected) when they’re removed from the display list. If a display object has objects subscribed as listeners to its events, that display object will not be removed from memory even when it’s removed from the display list, because it will still have references to those listener objects. For more information, see

“Managing event listeners” on page 138

.

### Choosing a DisplayObject subclass {#choosing-a-displayobject-subclass}

Flash Player 9 and later, Adobe AIR 1.0 and later

With several options to choose from, one of the important decisions you’ll make when you’re working with display objects is which display object to use for what purpose. Here are some guidelines to help you decide. These same suggestions apply whether you need an instance of a class or you’re choosing a base class for a class you’re creating:

*   If you don’t need an object that can be a container for other display objects (that is, you just need one that serves as a stand-alone screen element), choose one of these DisplayObject or InteractiveObject subclasses, depending on what it will be used for:
    *   Bitmap for displaying a bitmap image.
    *   TextField for adding text.
    *   Video for displaying video.
    *   Shape for a “canvas” for drawing content on-screen. In particular, if you want to create an instance for drawing shapes on the screen, and it won’t be a container for other display objects, you’ll gain significant performance benefits using Shape instead of Sprite or MovieClip.
    *   MorphShape, StaticText, or SimpleButton for items created by the Flash authoring tool. (You can’t create instances of these classes programmatically, but you can create variables with these data types to refer to items created using the Flash authoring tool.)
*   If you need a variable to refer to the main Stage, use the Stage class as its data type.
*   If you need a container for loading an external SWF file or image file, use a Loader instance. The loaded content will be added to the display list as a child of the Loader instance. Its data type will depend on the nature of the loaded content, as follows:
    *   A loaded image will be a Bitmap instance.
    *   A loaded SWF file written in ActionScript 3.0 will be a Sprite or MovieClip instance (or an instance of a subclass of those classes, as specified by the content creator).
    *   A loaded SWF file written in ActionScript 1.0 or ActionScript 2.0 will be an AVM1Movie instance.
*   If you need an object to serve as a container for other display objects (whether or not you’ll also be drawing onto the display object using ActionScript), choose one of the DisplayObjectContainer subclasses:
    *   Sprite if the object will be created using only ActionScript, or as the base class for a custom display object that will be created and manipulated solely with ActionScript.
    *   MovieClip if you’re creating a variable to refer to a movie clip symbol created in the Flash authoring tool.
*   If you are creating a class that will be associated with a movie clip symbol in the Flash library, choose one of these DisplayObjectContainer subclasses as your class’s base class:
    *   MovieClip if the associated movie clip symbol has content on more than one frame
    *   Sprite if the associated movie clip symbol has content only on the first frame