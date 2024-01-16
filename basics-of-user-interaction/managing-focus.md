# Managing focus {#managing-focus}

An interactive object can receive focus, either programmatically or through a user action. Additionally, if the tabEnabled property is set to true, the user can pass focus from one object to another by pressing the Tab key. Note that the tabEnabled value is false by default, except in the following cases:

• For a SimpleButton object, the value is true.

• For a input text field, the value is true.

• For a Sprite or MovieClip object with buttonMode set to true, the value is true.

In each of these situations, you can add a listener for FocusEvent.FOCUS_IN or FocusEvent.FOCUS_OUT to provide additional behavior when focus changes. This is particularly useful for text fields and forms, but can also be used on sprites, movie clips, or any object that inherits from the InteractiveObject class. The following example shows how to enable focus cycling with the Tab key and how to respond to the subsequent focus event. In this case, each square changes color as it receives focus.

**_Note:_** _Flash Professional uses keyboard shortcuts to manage focus; therefore, to properly simulate focus management, projects should be tested in a browser or AIR rather than within Flash._

var rows:uint = 10; var cols:uint = 10;

var rowSpacing:uint = 25; var colSpacing:uint = 25; var i:uint;

var j:uint;

for (i = 0; i &lt; rows; i++)

{

for (j = 0; j &lt; cols; j++)

{

createSquare(j * colSpacing, i * rowSpacing, (i * cols) + j);

}

}

function createSquare(startX:Number, startY:Number, tabNumber:uint):void

{

var square:Sprite = new Sprite(); square.graphics.beginFill(0x000000); square.graphics.drawRect(0, 0, colSpacing, rowSpacing); square.graphics.endFill();

square.x = startX;

square.y = startY; square.tabEnabled = true; square.tabIndex = tabNumber;

square.addEventListener(FocusEvent.FOCUS_IN, changeColor); addChild(square);

}

function changeColor(event:FocusEvent):void

{

event.target.transform.colorTransform = getRandomColor();

}

function getRandomColor():ColorTransform

{

// Generate random values for the red, green, and blue color channels. var red:Number = (Math.random() * 512) - 255;

var green:Number = (Math.random() * 512) - 255; var blue:Number = (Math.random() * 512) - 255;

// Create and return a ColorTransform object with the random colors. return new ColorTransform(1, 1, 1, 1, red, green, blue, 0);

}