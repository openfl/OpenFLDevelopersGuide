# Virtual keyboards {#virtual-keyboards}

OpenFL 10.2 and later, AIR 2.6 and later

Mobile devices, such as phones and tablets, often provide a virtual, software keyboard instead of a physical one. The classes in the Flash API let you:

• Detect when the virtual keyboard is raised and when it closes.

• Prevent the keyboard from raising.

• Determine the area of the stage obscured by the virtual keyboard.

• Create interactive objects that raise the keyboard when they gain focus. (Not supported by AIR applications on iOS.)

• (AIR only) Disable the automatic panning behavior so that your application can modify its own display to accommodate the keyboard.

**Controlling virtual keyboard behavior**

The runtime automatically opens the virtual keyboard when the user taps inside a text field or a specially configured interactive object. When the keyboard opens, the runtime follows the native platform conventions in panning and resizing the application content so that the user can see the text as they type.

When the keyboard opens, the focused object dispatches the following events in sequence:

softKeyboardActivating event — dispatched immediately before the keyboard begins to rise over the stage. If you call the preventDefault() method of the dispatched event object, the virtual keyboard does not open.

softKeyboardActivate event — dispatched after softKeyboardActivating event handling has completed. When the focused object dispatches this event, the softKeyboardRect property of the Stage object has been updated to reflect the area of the stage obscured by the virtual keyboard. This event cannot be canceled.

**_Note:_ **_If the keyboard changes size, for example, when the user changes the keyboard type, the focused object dispatches a second softKeyboardActivate event._

softKeyboardDeactivate event — dispatched when the virtual keyboard closes for any reason. This event cannot be canceled.

The following example adds two TextField objects on the stage. The upper TextField prevents the keyboard from raising when you tap the field and closes it if it is already raised. The lower TextField demonstrates the default behavior. The example reports the soft keyboard events dispatched by both text fields.

package

{

import flash.display.Sprite; import flash.text.TextField; import flash.text.TextFieldType;

import flash.events.SoftKeyboardEvent;

public class SoftKeyboardEventExample extends Sprite

{

private var tf1:TextField = new TextField(); private var tf2:TextField = new TextField();

public function SoftKeyboardEventExample()

{

tf1.width = this.stage.stageWidth; tf1.type = TextFieldType.INPUT; tf1.border = true;

this.addChild( tf1 );

tf1.addEventListener( SoftKeyboardEvent.SOFT_KEYBOARD_ACTIVATING, preventSoftKe yboard );

tf1.addEventListener( SoftKeyboardEvent.SOFT_KEYBOARD_ACTIVATE, preventSoftKe yboard );

tf1.addEventListener( SoftKeyboardEvent.SOFT_KEYBOARD_DEACTIVATE, preventSoftKeyboard

);

tf2.border = true;

tf2.type = TextFieldType.INPUT;

tf2.width = this.stage.stageWidth; tf2.y = tf1.y + tf1.height + 30; this.addChild( tf2 );

tf2.addEventListener( SoftKeyboardEvent.SOFT_KEYBOARD_ACTIVATING, allowSoftKeyboard ); tf2.addEventListener( SoftKeyboardEvent.SOFT_KEYBOARD_ACTIVATE, allowSoftKeyboard ); tf2.addEventListener( SoftKeyboardEvent.SOFT_KEYBOARD_DEACTIVATE, allowSoftKeyboard);

}

private function preventSoftKeyboard( event:SoftKeyboardEvent ):void

{

event.preventDefault();

this.stage.focus = null; //close the keyboard, if raised

trace( &quot;tf1 dispatched: &quot; + event.type + &quot; -- &quot; + event.triggerType );

}

private function allowSoftKeyboard( event:SoftKeyboardEvent ) :void

{

trace( &quot;tf2 dispatched: &quot; + event.type + &quot; -- &quot; + event.triggerType );

}

}

}

## Adding virtual keyboard support for interactive objects {#adding-virtual-keyboard-support-for-interactive-objects}

OpenFL 10.2 and later, AIR 2.6 and later (but not supported on iOS)

Normally, the virtual keyboard only opens when a TextField object is tapped. You can configure an instance of the InteractiveObject class to open the virtual keyboard when it receives focus.

To configure an InteractiveObject instance to open the soft keyboard, set its needsSoftKeyboard property to true. Whenever the object is assigned to the stage focus property, the soft keyboard automatically opens. In addition, you can raise the keyboard by calling the requestSoftKeyboard() method of the InteractiveObject.

The following example illustrates how you can program an InteractiveObject to act as a text entry field. The TextInput class shown in the example sets the needsSoftKeyboard property so that the keyboard is raised when needed. The object then listens for keyDown events and inserts the typed character into the field.

The example uses the Flash text engine to append and display any typed text and handles some important events. For simplicity, the example does not implement a full-featured text field.

package {

import flash.geom.Rectangle; import flash.display.Sprite;

import flash.text.engine.TextElement; import flash.text.engine.TextBlock; import flash.events.MouseEvent; import flash.events.FocusEvent; import flash.events.KeyboardEvent; import flash.text.engine.TextLine;

import flash.text.engine.ElementFormat; import flash.events.Event;

public class TextInput extends Sprite

{

public var text:String = &quot; &quot;; public var textSize:Number = 24;

public var textColor:uint = 0x000000;

private var _bounds:Rectangle = new Rectangle( 0, 0, 100, textSize ); private var textElement: TextElement;

private var textBlock:TextBlock = new TextBlock();

public function TextInput( text:String = &quot;&quot; )

{

this.text = text; this.scrollRect = _bounds; this.focusRect= false;

//Enable keyboard support this.needsSoftKeyboard = true;

this.addEventListener(MouseEvent.MOUSE_DOWN, onSelect); this.addEventListener(FocusEvent.FOCUS_IN, onFocusIn); this.addEventListener(FocusEvent.FOCUS_OUT, onFocusOut);

//Setup text engine

textElement = new TextElement( text, new ElementFormat( null, textSize, textColor ) ); textBlock.content = textElement;

var firstLine:TextLine = textBlock.createTextLine( null, _bounds.width - 8 ); firstLine.x = 4;

firstLine.y = 4 + firstLine.totalHeight; this.addChild( firstLine );

}

private function onSelect( event:MouseEvent ):void

{

stage.focus = this;

}

private function onFocusIn( event:FocusEvent ):void

{

this.addEventListener( KeyboardEvent.KEY_DOWN, onKey );

}

private function onFocusOut( event:FocusEvent ):void

{

this.removeEventListener( KeyboardEvent.KEY_UP, onKey );

}

private function onKey( event:KeyboardEvent ):void

{

textElement.replaceText( textElement.text.length, textElement.text.length, String.fromCharCode( event.charCode ) );

updateText();

}

public function set bounds( newBounds:Rectangle ):void

{

_bounds = newBounds.clone(); drawBackground(); updateText(); this.scrollRect = _bounds;

//force update to focus rect, if needed

if( this.stage!= null &amp;&amp; this.focusRect &amp;&amp; this.stage.focus == this ) this.stage.focus = this;

}

private function updateText():void

{

//clear text lines

while( this.numChildren &gt; 0 ) this.removeChildAt( 0 );

//and recreate them

var textLine:TextLine = textBlock.createTextLine( null, _bounds.width - 8); while ( textLine)

{

textLine.x = 4;

if( textLine.previousLine != null )

{

textLine.y = textLine.previousLine.y +

textLine.previousLine.totalHeight + 2;

}

else

{

textLine.y = 4 + textLine.totalHeight;

}

this.addChild(textLine);

textLine = textBlock.createTextLine(textLine, _bounds.width - 8 );

}

}

private function drawBackground():void

{

//draw background and border for the field this.graphics.clear(); this.graphics.beginFill( 0xededed ); this.graphics.lineStyle( 1, 0x000000 );

this.graphics.drawRect( _bounds.x + 2, _bounds.y + 2, _bounds.width - 4,

_bounds.height - 4);

this.graphics.endFill();

}

}

}

The following main application class illustrates how to use the TextInput class and manage the application layout when the keyboard is raised or the device orientation changes. The main class creates a TextInput object and sets its bounds to fill the stage. The class adjusts the size of the TextInput object when either the soft keyboard is raised or the stage changes size. The class listens for soft keyboard events from the TextInput object and resize events from the stage.

Irrespective of the cause of the event, the application determines the visible area of the stage and resizes the input control to fill it. Naturally, in a real application, you would need a more sophisticated layout algorithm.

package {

import flash.display.MovieClip;

import flash.events.SoftKeyboardEvent; import flash.geom.Rectangle;

import flash.events.Event;

import flash.display.StageScaleMode; import flash.display.StageAlign;

public class CustomTextField extends MovieClip {

private var customField:TextInput = new TextInput(&quot;Input text: &quot;); public function CustomTextField() {

this.stage.scaleMode = StageScaleMode.NO_SCALE; this.stage.align = StageAlign.TOP_LEFT; this.addChild( customField );

customField.bounds = new Rectangle( 0, 0, this.stage.stageWidth, this.stage.stageHeight );

//track soft keyboard and stage resize events customField.addEventListener(SoftKeyboardEvent.SOFT_KEYBOARD_ACTIVATE,

onDisplayAreaChange );

customField.addEventListener(SoftKeyboardEvent.SOFT_KEYBOARD_DEACTIVATE, onDisplayAreaChange );

this.stage.addEventListener( Event.RESIZE, onDisplayAreaChange );

}

private function onDisplayAreaChange( event:Event ):void

{

//Fill the stage if possible, but avoid the area covered by a keyboard var desiredBounds = new Rectangle( 0, 0, this.stage.stageWidth,

this.stage.stageHeight );

if( this.stage.stageHeight - this.stage.softKeyboardRect.height &lt; desiredBounds.height )

desiredBounds.height = this.stage.stageHeight - this.stage.softKeyboardRect.height;

customField.bounds = desiredBounds;

}

}

}

**_Note:_ **_The stage only dispatches resize events in response to an orientation change when the scaleMode property is set to_

_noScale. In other modes, the dimensions of the stage do not change; instead, the content is scaled to compensate._

## Handling application display changes {#handling-application-display-changes}

AIR 2.6 and later

In AIR, you can turn off the default panning and resizing behavior associated with raising a soft keyboard by setting the softKeyboardBehavior element in the application descriptor to none:

&lt;softKeyboardBehavior&gt;none&lt;/softKeyboardBehavior&gt;

When you turn off the automatic behavior, it is your application’s responsibility to make any necessary adjustments to the application display. A softKeyboardActivate event is dispatched when the keyboard opens. When the softKeyboardActivate event is dispatched, the softKeyboardRect property of the stage contains the dimensions of the area obscured by the open keyboard. Use these dimensions to move or resize your content so that it is displayed properly while the keyboard is open and the user is typing. (When the keyboard is closed, the dimensions of the softKeyboardRect rectangle are all zero.)

When the keyboard closes, a softKeyboardDeactivate event is dispatched, and you can return the application display to normal.

package {

import flash.display.MovieClip;

import flash.events.SoftKeyboardEvent; import flash.events.Event;

import flash.display.StageScaleMode; import flash.display.StageAlign;

import flash.display.InteractiveObject; import flash.text.TextFieldType;

import flash.text.TextField;

public class PanningExample extends MovieClip { private var textField:TextField = new TextField(); public function PanningExample() {

this.stage.scaleMode = StageScaleMode.NO_SCALE; this.stage.align = StageAlign.TOP_LEFT;

textField.y = this.stage.stageHeight - 201; textField.width = this.stage.stageWidth; textField.height = 200;

textField.type = TextFieldType.INPUT; textField.border = true; textField.wordWrap = true; textField.multiline = true;

this.addChild( textField );

//track soft keyboard and stage resize events textField.addEventListener(SoftKeyboardEvent.SOFT_KEYBOARD_ACTIVATE,

onKeyboardChange );

textField.addEventListener(SoftKeyboardEvent.SOFT_KEYBOARD_DEACTIVATE, onKeyboardChange );

this.stage.addEventListener( Event.RESIZE, onDisplayAreaChange );

}

private function onDisplayAreaChange( event:Event ):void

{

textField.y = this.stage.stageHeight - 201; textField.width = this.stage.stageWidth;

}

private function onKeyboardChange( event:SoftKeyboardEvent ):void

{

var field:InteractiveObject = textField; var offset:int = 0;

//if the softkeyboard is open and the field is at least partially covered if( (this.stage.softKeyboardRect.y != 0) &amp;&amp; (field.y + field.height &gt;

this.stage.softKeyboardRect.y) )

offset = field.y + field.height - this.stage.softKeyboardRect.y;

//but don&#039;t push the top of the field above the top of the screen if( field.y - offset &lt; 0 ) offset += field.y - offset;

this.y = -offset;

}

}

}

**_Note:_ **_On Android, there are circumstances, including fullscreen mode, in which the exact dimensions of the keyboard are not available from the operating system. In these cases, the size is estimated. Also, in landscape orientations, the native fullscreen IME keyboard is used for all text entry. This IME keyboard has a built-in text entry field and obscures the entire stage. There is no way to display a landscape keyboard that does not fill the screen._