# Touch and drag {#touch-and-drag}

OpenFL 10.1 and later, Adobe AIR 2 and later

Two methods were added to [the Sprite class](https://api.openfl.org/openfl/display/Sprite.html) to provide additional support for touch-enabled applications supporting touch-point input: Sprite.startTouchDrag() and Sprite.stopTouchDrag(). These methods behave the same as Sprite.startDrag() and Sprite.stopDrag() do for mouse events. However, notice the Sprite.startTouchDrag() and Sprite.stopTouchDrag() methods both take touchPointID values as parameters.

The runtime assigns the touchPointID value to the event object for a touch event. Use this value to respond to a specific touch point in the case the environment supports multiple, simultaneous, touch points (even if it does not handle gestures). For more information about the touchPointID property, see

"Touch Point ID" on page 587

.

The following code shows a simple start drag event handler and a stop drag event handler for a touch event. The variable _bg_ is a display object that contains _mySprite_:

mySprite.addEventListener(TouchEvent.TOUCH_BEGIN, onTouchBegin); mySprite.addEventListener(TouchEvent.TOUCH_END, onTouchEnd);

function onTouchBegin(e:TouchEvent) { e.target.startTouchDrag(e.touchPointID, false, bg.getRect(this)); trace(&quot;touch begin&quot;);

}

function onTouchEnd(e:TouchEvent) { e.target.stopTouchDrag(e.touchPointID); trace(&quot;touch end&quot;);

}

And the following shows a more advanced example combining dragging with touch event phases:

Multitouch.inputMode = MultitouchInputMode.TOUCH_POINT; var mySprite:Sprite = new Sprite();

mySprite.graphics.beginFill(0x336699); mySprite.graphics.drawRect(0,0,40,40); addChild(mySprite);

mySprite.addEventListener(TouchEvent.TOUCH_BEGIN, onTouchBegin); mySprite.addEventListener(TouchEvent.TOUCH_MOVE, onTouchMove); mySprite.addEventListener(TouchEvent.TOUCH_END, onTouchEnd);

function onTouchBegin(evt:TouchEvent) { evt.target.startTouchDrag(evt.touchPointID); evt.target.scaleX *= 1.5;

evt.target.scaleY *= 1.5;

}

function onTouchMove(evt:TouchEvent) { evt.target.alpha = 0.5;

}

function onTouchEnd(evt:TouchEvent) { evt.target.stopTouchDrag(evt.touchPointID); evt.target.width = 40;

evt.target.height = 40;

evt.target.alpha = 1;

}