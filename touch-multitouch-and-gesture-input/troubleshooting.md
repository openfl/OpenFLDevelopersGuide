# Troubleshooting {#troubleshooting}

Hardware and software support for touch input changed rapidly over time. This
reference does not maintain a list of every device an operating system and
software combination that supports multitouch. However, it provides guidance on
using the discovery API to determine if your application is deployed on a device
that supports multitouch, and provides tips for troubleshooting your
OpenFL code.

OpenFL responds to touch events based on information the device,
operating system, or containing software (such as a browser) passes to the
runtime. This dependency on the software environment complicates documenting
multitouch compatibility. Some devices interpret a gesture or touch motion
differently than another device. Is rotation defined by two fingers rotating at
the same time? Is rotation one finger drawing a circle on a screen? Depending on
the hardware and software environment, the rotation gesture could be either, or
something entirely different. So, the device tells the operating system the user
input, then the operating system passes that information to the runtime. If the
runtime is inside a browser, the browser software sometimes interprets the
gesture or touch event and does not pass the input to the runtime. This behavior
is similar to the behavior of _keyboard shortcuts_ or _hotkeys_: you try to use a specific key
combination to get OpenFL to do something inside the browser and the
browser keeps opening a menu instead.

Individual API and classes mention if they're not compatible with specific
operating systems. You can explore individual API entries here, starting with
the Multitouch class:
<https://api.openfl.org/openfl/ui/Multitouch.html>.

Here are some common gesture and touch descriptions:

**Pan**  
Move a finger left-to-right or right-to-left. Some devices require two fingers
to pan.

**Rotate**  
Touch two fingers down, then move them around in a circle (as if they're both
simultaneously tracing an imaginary circle on a surface). The pivot point is set
at the midpoint between the two finger touch points.

**Swipe**  
Move three fingers left-to-right or right-to-left, top-to-bottom, or
bottom-to-top, quickly.

**Zoom**  
Touch two fingers down, then move them away from each other to zoom in and
toward each other to zoom out.

**Press-and-tap**  
Move or press one finger, then tap the surface with another.

Each device has its own documentation about the gestures the device supports and
how to perform each gesture on that device. In general, the user must remove all
fingers from contact with the device between gestures, depending upon the
operating system.

If you find your application is not responding to touch events or gestures, test
the following:

1.  Do you have event listeners for touch or gesture events attached to an
    object class that inherits from the InteractiveObject class? Only
    InteractiveObject instances can listen for touch and gesture events

2.  Start simple and see what does work, first (the following code example is
    from the API entry for `Multitouch.inputMode`:

    ```haxe
    Multitouch.inputMode = MultitouchInputMode.TOUCH_POINT;

    var mySprite:Sprite = new Sprite();
    mySprite.graphics.beginFill(0x336699);
    mySprite.graphics.drawRect(0,0,40,40);
    addChild(mySprite);

    var myTextField:TextField = new TextField();

    mySprite.addEventListener(TouchEvent.TOUCH_TAP, taplistener);

    function taplistener(e:TouchEvent):Void {
        myTextField.text = "I've been tapped";
        myTextField.y = 50;
        addChild(myTextField);
    }
    ```

    Tap the rectangle. If this example works, then you know your environment
    supports a simple tap. Then you can try more complicated handling.

    <!-- TODO: uncomment when gesture support is implemented
    Testing for gesture support is more complicated. An individual device or
    operating system supports any combination of gesture input, or none.

    Here is a simple test for the zoom gesture:

        Multitouch.inputMode = MultitouchInputMode.GESTURE;

        stage.addEventListener(TransformGestureEvent.GESTURE_ZOOM, onZoom);
        var myTextField = new TextField();
        myTextField.y = 200;
        myTextField.text = "Perform a zoom gesture";
        addChild(myTextField);

        function onZoom(evt:TransformGestureEvent):Coid {
        	myTextField.text = "Zoom is supported";
        }

    Perform a zoom gesture on the device and see if the text field populates
    with the message `Zoom is supported`. The event listener is added to the
    stage so you can perform the gesture on any part of the test application.

    Here is a simple test for the pan gesture:

        Multitouch.inputMode = MultitouchInputMode.GESTURE;

        stage.addEventListener(TransformGestureEvent.GESTURE_PAN, onPan);
        var myTextField = new TextField();
        myTextField.y = 200;
        myTextField.text = "Perform a pan gesture";
        addChild(myTextField);

        function onPan(evt:TransformGestureEvent):Coid {
        	myTextField.text = "Pan is supported";
        }

    Perform a pan gesture on the device and see if the text field populates with
    the message `Pan is supported`. The event listener is added to the stage so
    you can perform the gesture on any part of the test application.

    Some operating system and device combinations support both gestures, some
    support only one, some none. Test your application's deployment environment
    to be sure.-->
