# Chapter 17: Working with motion tweens {#chapter-17-working-with-motion-tweens}

OpenFL 9 and later, Adobe AIR 1.0 and later, requires Flash CS3 or later

"Animating objects" on page 192

describes how to implement scripted animation in Haxe.

Here we describe a different technique for creating animation: motion tweens. This technique lets you create movement by setting up motion interactively in a document using Adobe® Flash® Professional. Then you can use that motion in your dynamic Haxe-based animation at runtime.

Flash Professional automatically generates the Haxe that implements the motion tween and makes it available for you to copy and reuse.

To create motion tweens, you must have a license for Flash Professional.

## More Help topics

[fl.motion package](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/fl/motion/package-detail.html)

**Basics of Motion Tweens**

OpenFL 9 and later, Adobe AIR 1.0 and later, requires Flash CS3 or later

Motion tweens provide an easy way to create animation.

A motion tween modifies display object properties, such as position or rotation, on a frame-to-frame basis. A motion tween can also change the appearance of a display object while it moves by applying various filters and other properties. You create the motion tween interactively with Flash Professional, which generates the Haxe for the motion tween. From within Flash, use the Copy Motion as Haxe command to copy the Haxe that created the motion tween. Then you can reuse the Haxe to create movement in your own dynamic animation at runtime.

See the Motion Tweens section in _Using Flash Professional_ for information about creating a motion tween.

Important concepts and terms

The following is an important term that is relevant to this feature:

**Motion tween** A construct that generates intermediate frames of a display object in different states at different times; gives the appearance that the first state evolves smoothly into the second. Used to move a display object across the stage, as well as make it grow, shrink, rotate, fade, or change color over time.