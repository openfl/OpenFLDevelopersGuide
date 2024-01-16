# Incorporating motion tween scripts {#incorporating-motion-tween-scripts}

OpenFL 9 and later, Adobe AIR 1.0 and later, requires Flash CS3 or later

The header in the Haxe code that you copy from Flash lists all the modules required to support the motion tween.

**Motion tween classes**

OpenFL 9 and later, Adobe AIR 1.0 and later, requires Flash CS3 or later

The essential motion tween classes are the AnimatorFactory, MotionBase, and Motion classes from the fl.motion package. You could need additional classes, depending on the properties that the motion tween manipulates. For example, if the motion tween transforms or rotates the display object, import the appropriate openfl.geom classes. If it applies filters, import the openfl.filter classes. In Haxe, a motion tween is an instance of the Motion class. The Motion class stores a keyframe animation sequence that can be applied to a visual object. The animation data includes position, scale, rotation, skew, color, filters, and easing.

The following Haxe was copied from a motion tween that was created in Flash to animate a display object whose instance name is Symbol1_2\. It declares a variable for a MotionBase object named motion_Symbol1_2\. The MotionBase class is the parent of the Motion class.

var motion_Symbol1_2:MotionBase;

Then the script creates the Motion object:

motion_Symbol1_2 = new Motion();

## Motion object names {#motion-object-names}

OpenFL 9 and later, Adobe AIR 1.0 and later, requires Flash CS3 or later

In the previous case, Flash automatically generated the name motion_Symbol1_2 for the Motion object. It attached the prefix motion_ to the display object name. Thus, the automatically generated name is based on the instance name of the target object of the motion tween in Flash. The duration property of the Motion object indicates the total number of frames in the motion tween:

motion_Symbol1_2.duration = 200;

By default, Flash automatically names the display object instance whose motion tween is copied, if it does not already have an instance name.

When you reuse Haxe created by Flash in your own animation, you can keep the name that Flash automatically generates for the tween or you can substitute a different name. If you change the tween name, make sure that you change it throughout the script.

Alternately, in Flash you can assign a name of your choosing to the target object of the motion tween. Then create the motion tween and copy the script. Whichever naming approach you use, make sure that each Motion object in your Haxe code has a unique name.