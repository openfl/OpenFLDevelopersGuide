## Enumerating the screens {#enumerating-the-screens}

Adobe AIR 1.0 and later

You can enumerate the screens of the virtual desktop with the following screen methods and properties:

| **Method or Property** | **Description** |
| --- | --- |
| Screen.screens | Provides an array of Screen objects describing the available screens. The order of the array is not significant. |
| Screen.mainScreen | Provides a Screen object for the main screen. On Mac OS X, the main screen is the screen displaying the menu bar. On Windows, the main screen is the system-designated primary screen. |
| Screen.getScreensForRectangle() | Provides an array of Screen objects describing the screens intersected by the given rectangle. The rectangle passed to this method is in pixel coordinates on the virtual desktop. If no screens intersect the rectangle, then the array is empty. You can use this method to find out on which screens a window is displayed. |

Do not save the values returned by the Screen class methods and properties. The user or operating system can change the available screens and their arrangement at any time.

The following example uses the screen API to move a window between multiple screens in response to pressing the arrow keys. To move the window to the next screen, the example gets the screens array and sorts it either vertically or horizontally (depending on the arrow key pressed). The code then walks through the sorted array, comparing each screen to the coordinates of the current screen. To identify the current screen of the window, the example calls Screen.getScreensForRectangle(), passing in the window bounds.

package {

import flash.display.Sprite; import flash.display.Screen; import flash.events.KeyboardEvent; import flash.ui.Keyboard;

import flash.display.StageAlign; import flash.display.StageScaleMode;

public class ScreenExample extends Sprite

{

public function ScreenExample()

{

stage.align = StageAlign.TOP_LEFT; stage.scaleMode = StageScaleMode.NO_SCALE;

stage.addEventListener(KeyboardEvent.KEY_DOWN,onKey);

}

private function onKey(event:KeyboardEvent):void{ if(Screen.screens.length &gt; 1){

switch(event.keyCode){ case Keyboard.LEFT :

moveLeft(); break;

case Keyboard.RIGHT : moveRight(); break;

case Keyboard.UP : moveUp(); break;

case Keyboard.DOWN : moveDown(); break;

}

}

}

private function moveLeft():void{

var currentScreen = getCurrentScreen(); var left:Array = Screen.screens; left.sort(sortHorizontal);

for(var i:int = 0; i &lt; left.length - 1; i++){ if(left[i].bounds.left &lt; stage.nativeWindow.bounds.left){

stage.nativeWindow.x +=

left[i].bounds.left - currentScreen.bounds.left; stage.nativeWindow.y += left[i].bounds.top - currentScreen.bounds.top;

}

}

}

private function moveRight():void{

var currentScreen:Screen = getCurrentScreen(); var left:Array = Screen.screens; left.sort(sortHorizontal);

for(var i:int = left.length - 1; i &gt; 0; i--){ if(left[i].bounds.left &gt; stage.nativeWindow.bounds.left){

stage.nativeWindow.x +=

left[i].bounds.left - currentScreen.bounds.left; stage.nativeWindow.y += left[i].bounds.top - currentScreen.bounds.top;

}

}

}

private function moveUp():void{

var currentScreen:Screen = getCurrentScreen(); var top:Array = Screen.screens; top.sort(sortVertical);

for(var i:int = 0; i &lt; top.length - 1; i++){ if(top[i].bounds.top &lt; stage.nativeWindow.bounds.top){

stage.nativeWindow.x += top[i].bounds.left - currentScreen.bounds.left; stage.nativeWindow.y += top[i].bounds.top - currentScreen.bounds.top; break;

}

}

}

private function moveDown():void{

var currentScreen:Screen = getCurrentScreen();

var top:Array = Screen.screens; top.sort(sortVertical);

for(var i:int = top.length - 1; i &gt; 0; i--){ if(top[i].bounds.top &gt; stage.nativeWindow.bounds.top){

stage.nativeWindow.x += top[i].bounds.left - currentScreen.bounds.left; stage.nativeWindow.y += top[i].bounds.top - currentScreen.bounds.top; break;

}

}

}

private function sortHorizontal(a:Screen,b:Screen):int{ if (a.bounds.left &gt; b.bounds.left){

return 1;

} else if (a.bounds.left &lt; b.bounds.left){ return -1;

} else {return 0;}

}

private function sortVertical(a:Screen,b:Screen):int{ if (a.bounds.top &gt; b.bounds.top){

return 1;

} else if (a.bounds.top &lt; b.bounds.top){ return -1;

} else {return 0;}

}

private function getCurrentScreen():Screen{ var current:Screen;

var screens:Array = Screen.getScreensForRectangle(stage.nativeWindow.bounds); (screens.length &gt; 0) ? current = screens[0] : current = Screen.mainScreen; return current;

}

}

}