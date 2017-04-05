# Chapter 6: Working with movie clips {#chapter-6-working-with-movie-clips}

The MovieClip class is the core class for animation and movie clip symbols that you create in the Adobe® Flash® Professional or Adobe® Animate® development environment. It has all the behaviors and functionality of display objects, but with additional properties and methods for controlling its timeline.

**Basics of movie clips**

Movie clips are a key element for people who create animated content with the Flash authoring tool and want to control that content with Haxe code. Whenever you create a movie clip symbol in Flash, Flash adds the symbol to the library of that Flash document. By default, this symbol becomes an instance of the [MovieClip class](http://api.openfl.org/openfl/display/MovieClip.html), and as such has the properties and methods of the MovieClip class.

When an instance of a movie clip symbol is placed on the Stage, the movie clip automatically progresses through its timeline (if it has more than one frame) unless its playback is altered using Haxe code. It is this timeline that distinguishes the MovieClip class, allowing you to create animation through motion or shape tweens through the Flash authoring tool. By contrast, with a display object that is an instance of the Sprite class, you can create animation only by programmatically changing the object’s values.

In OpenFL, a movie clip is only one of many display objects that can appear on the screen. If a timeline is not necessary for the function of a display object, using the Shape class or Sprite class in lieu of the MovieClip class may improve rendering performance. For more information on choosing the appropriate display object for a task, see [Choosing a DisplayObject subclass](/display-programming/working-with-display-objects/choosing-a-displayobject-subclass.md).

### Important concepts and terms

The following reference list contains important terms related to movie clips:

**AVM1 SWF** A project created using ActionScript 1.0 or ActionScript 2.0, usually targeting Flash Player 8 or earlier.

**AVM2 SWF** A project created using Haxe or ActionScript 3.0 for Adobe Flash Player 9 or later.

**External SWF** A SWF file that is created separately from the current project and is intended to be loaded as an asset into the project, whether for Flash Player or on another OpenFL target platform.

**Frame** The smallest division of time on the timeline. As with a motion picture filmstrip, each frame is like a snapshot of the animation in time, and when frames are played quickly in sequence, the effect of animation is created.

**Timeline** The metaphorical representation of the series of frames that make up a movie clip’s animation sequence. The timeline of a MovieClip object is equivalent to the timeline in the Flash authoring tool.

**Playhead** A marker identifying the location (frame) in the timeline that is being displayed at a given moment.