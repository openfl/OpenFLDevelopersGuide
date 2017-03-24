# Understanding the Video class {#understanding-the-video-class}

The Video class enables you to display live streaming video in an application without embedding it in your project. You can capture and play live video using the Camera.getCamera() method. You can also use the Video class to play back video files over HTTP or from the local file system. There are several different ways to use Video in your projects:

• Load a video file dynamically using the NetConnection and NetStream classes and display the video in a Video object.

• Capture input from the user’s camera. For more information, see

“Working with cameras” on page 521

.

• Use the FLVPlayback component.

• Use the VideoDisplay control.

**_Note:_ **_Instances of a Video object on the Stage are instances of the Video class._

Even though the Video class is in the flash.media package, it inherits from the flash.display.DisplayObject class. Therefore, all display object functionality, such as matrix transformations and filters, also applies to Video instances.

For more information see

“Manipulating display objects” on page 173

,

“Working with geometry” on page 210

, and

“Filtering display objects” on page 267

.