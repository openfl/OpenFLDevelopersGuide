# Summary

* [Introduction](README.md)
* [Chapter 1: Handling events](handling-events\README.md)
  * [Basics of handling events](handling-events\basics-of-handling-events.md)
  * [The event flow](handling-events\the-event-flow.md)
  * [Event objects](handling-events\event-objects.md)
  * [Event listeners](handling-events\event-listeners.md)
  #* [Event handling example: Alarm Clock](handling-events\event-handling-example-alarm-clock.md)
* [Chapter 2: Display programming](display-programming\README.md)
  * [Basics of display programming](display-programming\basics-of-display-programming.md)
  * [Core display classes](display-programming\core-display-classes.md)
  * [Advantages of the display list approach](display-programming\advantages-of-the-display-list-approach.md)
  * [Working with display objects](display-programming\working-with-display-objects\README.md)
    * [Properties and methods of the DisplayObject class](display-programming\working-with-display-objects\properties-and-methods-of-the-displayobject-class.md)
    * [Adding display objects to the display list](display-programming\working-with-display-objects\adding-display-objects-to-the-display-list.md)
    * [Working with display object containers](display-programming\working-with-display-objects\working-with-display-object-containers.md)
    * [Traversing the display list](display-programming\working-with-display-objects\traversing-the-display-list.md)
    * [Setting Stage properties](display-programming\working-with-display-objects\setting-stage-properties.md)
    * [Handling events for display objects](display-programming\working-with-display-objects\handling-events-for-display-objects.md)
    * [Choosing a DisplayObject subclass](display-programming\working-with-display-objects\choosing-a-displayobject-subclass.md)
  * [Manipulating display objects](display-programming\manipulating-display-objects\README.md)
    * [Changing position](display-programming\manipulating-display-objects\changing-position.md)
    * [Panning and scrolling display objects](display-programming\manipulating-display-objects\panning-and-scrolling-display-objects.md)
    * [Manipulating size and scaling objects](display-programming\manipulating-display-objects\manipulating-size-and-scaling-objects.md)
    * [Caching display objects](display-programming\manipulating-display-objects\caching-display-objects.md)
    * [Setting an opaque background color](display-programming\manipulating-display-objects\setting-an-opaque-background-color.md)
    * [Applying blending modes](display-programming\manipulating-display-objects\applying-blending-modes.md)
    * [Adjusting DisplayObject colors](display-programming\manipulating-display-objects\adjusting-displayobject-colors.md)
    * [Rotating objects](display-programming\manipulating-display-objects\rotating-objects.md)
    * [Fading objects](display-programming\manipulating-display-objects\fading-objects.md)
    * [Masking display objects](display-programming\manipulating-display-objects\masking-display-objects.md)
  * [Animating objects](display-programming\animating-objects.md)
  #* [Stage orientation](display-programming\stage-orientation.md)
  * [Loading display content dynamically](display-programming\loading-display-content-dynamically\README.md)
    * [Loading display objects](display-programming\loading-display-content-dynamically\loading-display-objects.md)
    * [Monitoring loading progress](display-programming\loading-display-content-dynamically\monitoring-loading-progress.md)
    * [Specifying loading context](display-programming\loading-display-content-dynamically\specifying-loading-context.md)
  #* [Display object example: SpriteArranger](display-programming\display-object-example-spritearranger.md)
* [Chapter 3: Working with geometry](working-with-geometry\README.md)
  * [Basics of geometry](working-with-geometry\basics-of-geometry.md)
  * [Using Point objects](working-with-geometry\using-point-objects.md)
  * [Using Rectangle objects](working-with-geometry\using-rectangle-objects.md)
  * [Using Matrix objects](working-with-geometry\using-matrix-objects.md)
  #* [Geometry example: Applying a matrix transformation to a display object](working-with-geometry\geometry-example-applying-a-matrix-transformation-.md)
* [Chapter 4: Using the drawing API](using-the-drawing-api\README.md)
  * [Basics of the drawing API](using-the-drawing-api\basics-of-the-drawing-api.md)
  * [Understanding the Graphics class](using-the-drawing-api\understanding-the-graphics-class.md)
  * [Drawing lines and curves](using-the-drawing-api\drawing-lines-and-curves.md)
  * [Drawing shapes using built-in methods](using-the-drawing-api\drawing-shapes-using-built-in-methods.md)
  * [Creating gradient lines and fills](using-the-drawing-api\creating-gradient-lines-and-fills.md)
  * [Using the Math class with drawing methods](using-the-drawing-api\using-the-math-class-with-drawing-methods.md)
  * [Animating with the drawing API](using-the-drawing-api\animating-with-the-drawing-api.md)
  #* [Drawing API example: Algorithmic Visual Generator](using-the-drawing-api\drawing-api-example-algorithmic-visual-generator.md)
  * [Advanced use of the drawing API](using-the-drawing-api\advanced-use-of-the-drawing-api\README.md)
    * [Drawing paths](using-the-drawing-api\advanced-use-of-the-drawing-api\drawing-paths.md)
    * [Defining winding rules](using-the-drawing-api\advanced-use-of-the-drawing-api\defining-winding-rules.md)
    * [Using graphics data classes](using-the-drawing-api\advanced-use-of-the-drawing-api\using-graphics-data-classes.md)
    * [About using drawTriangles()](using-the-drawing-api\advanced-use-of-the-drawing-api\about-using-drawtriangles.md)
* [Chapter 5: Working with bitmaps](working-with-bitmaps\README.md)
  * [Basics of working with bitmaps](working-with-bitmaps\basics-of-working-with-bitmaps.md)
  * [The Bitmap and BitmapData classes](working-with-bitmaps\the-bitmap-and-bitmapdata-classes.md)
  * [Manipulating pixels](working-with-bitmaps\manipulating-pixels.md)
  * [Copying bitmap data](working-with-bitmaps\copying-bitmap-data.md)
  * [Compressing bitmap data](working-with-bitmaps\compressing-bitmap-data.md)
  * [Making textures with noise functions](working-with-bitmaps\making-textures-with-noise-functions.md)
  * [Scrolling bitmaps](working-with-bitmaps\scrolling-bitmaps.md)
  * [Working with bitmap assets](working-with-bitmaps\working-with-bitmap-assets.md)
#  * [Taking advantage of mipmapping](working-with-bitmaps\taking-advantage-of-mipmapping.md)
#  * [Bitmap example: Animated spinning moon](working-with-bitmaps\bitmap-example-animated-spinning-moon.md)
#  * [Asynchronous decoding of bitmap images](working-with-bitmaps\asynchronous-decoding-of-bitmap-images.md)
#* [Chapter 14: Filtering display objects](filtering-display-objects\README.md)
#  * [Creating and applying filters](filtering-display-objects\creating-and-applying-filters.md)
#  * [Available display filters](filtering-display-objects\available-display-filters.md)
#  * [Filtering display objects example: Filter Workbench](filtering-display-objects\filtering-display-objects-example-filter-workbench.md)
* [Chapter 6: Working with movie clips](working-with-movie-clips\README.md)
  * [Basics of movie clips](working-with-movie-clips\basics-of-movie-clips.md)
  * [Working with MovieClip objects](working-with-movie-clips\working-with-movieclip-objects.md)
  * [Controlling movie clip playback](working-with-movie-clips\controlling-movie-clip-playback.md)
  * [Creating MovieClip objects with Haxe](working-with-movie-clips\creating-movieclip-objects-with-haxe.md)
#  * [Loading an external project](working-with-movie-clips\loading-an-external-swf-file.md)
#  * [Movie clip example:  RuntimeAssetsExplorer](working-with-movie-clips\movie-clip-example-runtimeassetsexplorer.md)
#* [Chapter 17: Working with motion tweens](working-with-motion-tweens\README.md)
#  * [Copying motion tween scripts in Flash](working-with-motion-tweens\copying-motion-tween-scripts-in-flash.md)
#  * [Incorporating motion tween scripts](working-with-motion-tweens\incorporating-motion-tween-scripts.md)
#  * [Describing the animation](working-with-motion-tweens\describing-the-animation.md)
#  * [Adding filters](working-with-motion-tweens\adding-filters.md)
#  * [Associating a motion tween with its display objects](working-with-motion-tweens\associating-a-motion-tween-with-its-display-object.md)
#* [Chapter 20: Basics of Working with text](basics-of-working-with-text\README.md)
* [Chapter 21: Using the TextField class](using-the-textfield-class\README.md)
  * [Displaying text](using-the-textfield-class\displaying-text.md)
  * [Selecting and manipulating text](using-the-textfield-class\selecting-and-manipulating-text.md)
  * [Capturing text input](using-the-textfield-class\capturing-text-input.md)
  * [Restricting text input](using-the-textfield-class\restricting-text-input.md)
  * [Formatting text](using-the-textfield-class\formatting-text.md)
  * [Advanced text rendering](using-the-textfield-class\advanced-text-rendering.md)
  * [Working with static text](using-the-textfield-class\working-with-static-text.md)
  * [Working with font assets](using-the-textfield-class\working-with-font-assets.md)
#  * [TextField Example: Newspaper-style text formatting](using-the-textfield-class\textfield-example-newspaper-style-text-formatting.md)
* [Chapter 24: Working with sound](working-with-sound\README.md)
  * [Basics of working with sound](working-with-sound\basics-of-working-with-sound.md)
  * [Understanding the sound architecture](working-with-sound\understanding-the-sound-architecture.md)
  * [Loading external sound files](working-with-sound\loading-external-sound-files.md)
  * [Working with sound assets](working-with-sound\working-with-sound-assets.md)
#  * [Working with streaming sound files](working-with-sound\working-with-streaming-sound-files.md)
  * [Working with dynamically generated audio](working-with-sound\working-with-dynamically-generated-audio.md)
  * [Playing sounds](working-with-sound\playing-sounds.md)
#  * [Security considerations when loading and playing sounds](working-with-sound\security-considerations-when-loading-and-playing-s.md)
  * [Controlling sound volume and panning](working-with-sound\controlling-sound-volume-and-panning.md)
#  * [Working with sound metadata](working-with-sound\working-with-sound-metadata.md)
#  * [Accessing raw sound data](working-with-sound\accessing-raw-sound-data.md)
#  * [Capturing sound input](working-with-sound\capturing-sound-input.md)
#  * [Sound example: Podcast Player](working-with-sound\sound-example-podcast-player.md)
#* [Chapter 25: Working with video](working-with-video\README.md)
#  * [Understanding video formats](working-with-video\understanding-video-formats.md)
#  * [Understanding the Video class](working-with-video\understanding-the-video-class.md)
#  * [Loading video files](working-with-video\loading-video-files.md)
#  * [Controlling video playback](working-with-video\controlling-video-playback.md)
#  * [Playing video in full-screen mode](working-with-video\playing-video-in-full-screen-mode.md)
#  * [Streaming video files](working-with-video\streaming-video-files.md)
#  * [Understanding cue points](working-with-video\understanding-cue-points.md)
#  * [Writing callback methods for metadata and cue points](working-with-video\writing-callback-methods-for-metadata-and-cue-poin.md)
#  * [Using cue points and metadata](working-with-video\using-cue-points-and-metadata.md)
#  * [Monitoring  NetStream activity](working-with-video\monitoring-netstream-activity.md)
#  * [Advanced topics for video files](working-with-video\advanced-topics-for-video-files.md)
#  * [Video example: Video Jukebox](working-with-video\video-example-video-jukebox.md)
#  * [Using the StageVideo class for hardware accelerated presentation](working-with-video\using-the-stagevideo-class-for-hardware-accelerate.md)
#* [Chapter 29: Basics of user interaction](basics-of-user-interaction\README.md)
#  * [Managing focus](basics-of-user-interaction\managing-focus.md)
#  * [Discovering input types](basics-of-user-interaction\discovering-input-types.md)
* [Chapter 30: Keyboard input](keyboard-input\README.md)
  * [Capturing keyboard input](keyboard-input\capturing-keyboard-input.md)
#  * [Using the IME class](keyboard-input\using-the-ime-class.md)
#  * [Virtual keyboards](keyboard-input\virtual-keyboards.md)
* [Chapter 31: Mouse input](mouse-input\README.md)
  * [Capturing mouse input](mouse-input\capturing-mouse-input.md)
#  * [Mouse input example: WordSearch](mouse-input\mouse-input-example-wordsearch.md)
* [Chapter 32: Touch, multitouch and gesture input](touch-multitouch-and-gesture-input\README.md)
  * [Touch support discovery](touch-multitouch-and-gesture-input\touch-support-discovery.md)
  * [Touch event handling](touch-multitouch-and-gesture-input\touch-event-handling.md)
  * [Troubleshooting](touch-multitouch-and-gesture-input\troubleshooting.md)
#  * [Touch and drag](touch-multitouch-and-gesture-input\touch-and-drag.md)
#  * [Gesture event handling](touch-multitouch-and-gesture-input\gesture-event-handling.md)
* [Chapter 33: Copy and paste](copy-and-paste\README.md)
  * [Reading from and writing to the system clipboard](copy-and-paste\reading-from-and-writing-to-the-system-clipboard.md)
#  * [HTML copy and paste in AIR](copy-and-paste\html-copy-and-paste-in-air.md)
#  * [Clipboard data formats](copy-and-paste\clipboard-data-formats.md)
* [Chapter 34: Accelerometer input](accelerometer-input\README.md)
  * [Detecting accelerometer changes](accelerometer-input\detecting-accelerometer-changes.md)
* [Chapter 38: Working with the file system](working-with-the-file-system\README.md)
  * [Using the FileReference class](working-with-the-file-system\using-the-filereference-class.md)
  * [Using the native file system API](working-with-the-file-system\using-the-native-file-system-api.md)
    * [Native file system basics](working-with-the-file-system\native-file-system-basics.md)
    * [Working with File objects in OpenFL](working-with-the-file-system\working-with-file-objects-in-openfl.md)
    * [Getting file system information](working-with-the-file-system\getting-file-system-information.md)
    * [Working with directories](working-with-the-file-system\working-with-directories.md)
    * [Working with files](working-with-the-file-system\working-with-files.md)
    * [Workflow for reading and writing files](working-with-the-file-system\workflow-for-reading-and-writing-files.md)
    * [FileStream open modes](working-with-the-file-system\filestream-open-modes.md)
    * [Initializing a FileStream object, and opening and closing files](working-with-the-file-system\initializing-a-filestream-object-and-opening-and-closing-files.md)
    * [The position property of a FileStream object](working-with-the-file-system\the-position-property-of-a-filestream-object.md)
    * [The read buffer and the bytesAvailable property of a FileStream object](working-with-the-file-system\the-read-buffer-and-the-bytesavailable-property-of-a-filestream-object.md)
    * [Asynchronous programming and the events generated by a FileStream object opened asynchronously](working-with-the-file-system\asynchronous-programming-and-the-events-generated-by-a-filestream-object-opened-asynchronously.md)
    * [Data formats, and choosing the read and write methods to use](working-with-the-file-system\data-formats-and-choosing-the-read-and-write-methods-to-use.md)
    * [Using the load() and save() methods](working-with-the-file-system\using-the-load-and-save-methods.md)
#* [Chapter 39: Storing local data](storing-local-data\README.md)
#  * [Encrypted local storage](storing-local-data\encrypted-local-storage.md)
* [Chapter 41: Working with byte arrays](working-with-byte-arrays\README.md)
  * [Reading a writing a byte array](working-with-byte-arrays\reading-and-writing-a-byte-array.md)
  * [Working with byte array assets](working-with-byte-arrays\working-with-byte-array-assets.md)
#  * [ByteArray example: Reading a .zip file](working-with-byte-arrays\bytearray-example-reading-a-zip-file.md)
#* [Chapter 42: Basics of networking and communication](basics-of-networking-and-communication\README.md)
#  * [Network connectivity changes](basics-of-networking-and-communication\network-connectivity-changes.md)
#  * [Domain Name System (DNS) records](basics-of-networking-and-communication\domain-name-system-dns-records.md)
#* [Chapter 43: Sockets](sockets\README.md)
#  * [UDP sockets (AIR)](sockets\udp-sockets-air.md)
#  * [IPv6 addresses](sockets\ipvaddresses.md)
* [Chapter 44: HTTP communications](http-communications\README.md)
  * [Loading external data](http-communications\loading-external-data.md)
  * [Web service requests](http-communications\web-service-requests.md)
    * [REST-style web service requests](http-communications\rest-style-web-service-requests.md)
    * [XML-RPC web service requests](http-communications\xml-rpc-web-service-requests.md)
    * [SOAP web service requests](http-communications\soap-web-service-requests.md)
#  * [Opening a URL in another application](http-communications\opening-a-url-in-another-application.md)
#* [Chapter 47: Using the external API](using-the-external-api\README.md)
#  * [External API requirements and advantages](using-the-external-api\external-api-requirements-and-advantages.md)
#  * [Using the ExternalInterface class](using-the-external-api\using-the-externalinterface-class.md)
#  * [External API example: Communicating between Haxe and JavaScript in a web browser](using-the-external-api\external-api-example-communicating-between-actions.md)
#* [Chapter 49: Client system environment](client-system-environment\README.md)
#  * [Using the System class](client-system-environment\using-the-system-class.md)
#  * [Using the Capabilities class](client-system-environment\using-the-capabilities-class.md)
#  * [Capabilities example: Detecting system capabilities](client-system-environment\capabilities-example-detecting-system-capabilities.md)
#* [Chapter 63: Using workers for concurrency](using-workers-for-concurrency\README.md)
#  * [Creating and managing workers](using-workers-for-concurrency\creating-and-managing-workers.md)
#  * [Communicating  between workers](using-workers-for-concurrency\communicating-between-workers.md)
#* [Chapter 68: Adobe Graphics Assembly Language (AGAL)](adobe-graphics-assembly-language-agal\README.md)
#  * [AGAL bytecode format](adobe-graphics-assembly-language-agal\agal-bytecode-format.md)
