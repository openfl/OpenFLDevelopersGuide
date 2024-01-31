# Chapter 32: Touch, multitouch and gesture input {#chapter-32-touch-multitouch-and-gesture-input}

The touch event handling features of OpenFL include input from a single point of
contact or multiple points of contact on touch-enabled devices.

<!--And, the OpenFL handles events that combine multiple touch points with
movement to create a gesture.--> In other words, OpenFL interprets the following

types of input:

**Touch**  
input with a single point device such as a finger, stylus, or other tool on a
touch-enabled device. Some devices support multiple simultaneous points of
contact with a finger or stylus.

**Multitouch**  
input with more than one simultaneous point of contact.

<!-- TODO: uncomment when gesture events are implemented
**Gesture**
Input interpreted by a device or operating system in response to one or more
touch events. For example, a user rotates two fingers simultaneously, and the
device or operating system interprets that touch input as a rotation gesture.
Some gestures are performed with one finger or touch point, and some gestures
require multiple touch points. The device or operating system establishes the
type of gesture to assign to the input.

Both touch and gesture input can be multitouch input depending on the user's
device. OpenFL provides APIs for handling touch events, gesture events, and
individually tracked touch events for multitouch input.-->

Note: Listening for touch and gesture events can consume a significant amount of
processing resources (equivalent to rendering several frames per second),
depending on the computing device and operating system. It is often better to
use mouse events when you do not actually need the extra functionality provided
by touch or gestures. When you do use touch or gesture events, consider reducing
the amount of graphical changes that can occur, especially when such events can
be dispatched rapidly, as during a pan, rotate, or zoom operation. For example,
you could stop animation within a component while the user resizes it using a
zoom gesture.

## Section Contents

- [Touch support discovery](./touch-support-discovery.md)
- [Touch event handling](./touch-event-handling.md)
- [Troubleshooting](./troubleshooting.md)

<!-- TODO: uncomment when startTouchDrag is implemented
- [Touch and drag](./touch-and-drag.md)->
<!-- TODO: uncomment when gesture events are implemented
- [Gesture event handling](./gesture-event-handling.md)-->

## More Help topics

- [openfl.ui.Multitouch](https://api.openfl.org/openfl/ui/Multitouch.html)
- [openfl.events.TouchEvent](https://api.openfl.org/openfl/events/TouchEvent.html)

<!-- TODO: uncomment when gesture events are implemented
- [openfl.events.GestureEvent](https://api.openfl.org/openfl/events/GestureEvent.html)
- [openfl.events.TransformGestureEvent](https://api.openfl.org/openfl/events/TransformGestureEvent.html)
- [openfl.events.GesturePhase](https://api.openfl.org/openfl/events/GesturePhase.html)
- [openfl.events.PressAndTapGestureEvent](https://api.openfl.org/openfl/events/PressAndTapGestureEvent.html)-->
