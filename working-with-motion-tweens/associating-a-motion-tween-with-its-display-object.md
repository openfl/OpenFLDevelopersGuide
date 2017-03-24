# Associating a motion tween with its display objects {#associating-a-motion-tween-with-its-display-objects}

OpenFL 9 and later, Adobe AIR 1.0 and later, requires Flash CS3 or later

The last task is to associate the motion tween with the display object or objects that it manipulates.

The AnimatorFactory class manages the association between a motion tween and its target display objects. The argument to the AnimatorFactory constructor is the Motion object:

var animFactory_Wheel:AnimatorFactory = new AnimatorFactory( motion_Wheel);

Use the addTarget() method of the AnimatorFactory class to associate the target display object with its motion tween. The Haxe copied from Flash comments out the addTarget() line and does not specify an instance name:

// animFactory_Wheel.addTarget(&lt;instance name goes here&gt;, 0);

In your copy, specify the display object to associate with the motion tween. In the following example, the targets are specified as greenWheel and redWheel:

animFactory_Wheel.AnimatorFactory.addTarget(greenWheel, 0);

animFactory_Wheel.AnimationFactory.addTarget(redWheel, 0);

You can associate multiple display objects with the same motion tween using multiple calls to addTarget().