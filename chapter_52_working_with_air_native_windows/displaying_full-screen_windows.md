## Displaying full-screen windows {#displaying-full-screen-windows}

Adobe AIR 1.0 and later

Setting the displayState property of the Stage to StageDisplayState.FULL_SCREEN_INTERACTIVE places the window in full-screen mode, and keyboard input _is_ permitted in this mode. (In SWF content running in a browser, keyboard input is not permitted). To exit full-screen mode, the user presses the Escape key.

**_Note:_ **_Some Linux window managers will not change the window dimensions to fill the screen if a maximum size is set for the window (but do remove the window system chrome)._

For example, the following Flex code defines a simple AIR application that sets up a simple full-screen terminal:

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) layout=&quot;vertical&quot;

applicationComplete=&quot;init()&quot; backgroundColor=&quot;0x003030&quot; focusRect=&quot;false&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

private function init():void

{

exit..\n&quot;;

}

]]&gt;

stage.displayState = StageDisplayState.FULL_SCREEN_INTERACTIVE; focusManager.setFocus(terminal);

terminal.text = &quot;Welcome to the dumb terminal app. Press the ESC key to

terminal.selectionBeginIndex = terminal.text.length; terminal.selectionEndIndex = terminal.text.length;

&lt;/mx:Script&gt;

&lt;mx:TextArea

id=&quot;terminal&quot; height=&quot;100%&quot; width=&quot;100%&quot; scroll=&quot;false&quot; backgroundColor=&quot;0x003030&quot; color=&quot;0xCCFF00&quot;

fontFamily=&quot;Lucida Console&quot; fontSize=&quot;44&quot;/&gt;

&lt;/mx:WindowedApplication&gt;

The following ActionScript example for Flash simulates a simple full-screen text terminal:

import flash.display.Sprite;

import flash.display.StageDisplayState; import flash.text.TextField;

import flash.text.TextFormat;

public class FullScreenTerminalExample extends Sprite

{

public function FullScreenTerminalExample():void

{

var terminal:TextField = new TextField(); terminal.multiline = true; terminal.wordWrap = true; terminal.selectable = true; terminal.background = true; terminal.backgroundColor = 0x00333333;

this.stage.displayState = StageDisplayState.FULL_SCREEN_INTERACTIVE; addChild(terminal);

terminal.width = 550;

terminal.height = 400;

terminal.text = &quot;Welcome to the dumb terminal application. Press the ESC key to

exit.\n_&quot;;

var tf:TextFormat = new TextFormat(); tf.font = &quot;Courier New&quot;;

tf.color = 0x00CCFF00; tf.size = 12; terminal.setTextFormat(tf);

terminal.setSelection(terminal.text.length - 1, terminal.text.length);

}

}