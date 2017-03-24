# Controlling video playback {#controlling-video-playback}

The NetStream class offers four main methods for controlling video playback:

[pause()](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/net/NetStream.html#pause()): Pauses playback of a video stream. If the video is already paused, calling this method does nothing.

[resume()](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/net/NetStream.html#resume()): Resumes playback of a video stream that is paused. If the video is already playing, calling this method does nothing.

[seek()](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/net/NetStream.html#seek()): Seeks the keyframe closest to the specified location (an offset, in seconds, from the beginning of the stream).

[togglePause()](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/net/NetStream.html#togglePause()): Pauses or resumes playback of a stream.

**_Note:_ **_There is no stop() method. In order to stop a stream you must pause playback and seek to the beginning of the video stream._

**_Note:_ **_The play() method does not resume playback, it is used for loading video files._

The following example demonstrates how to control a video using several different buttons. To run the following example, create a new document and add four button instances to your workspace (pauseBtn, playBtn, stopBtn, and togglePauseBtn):

var nc:NetConnection = new NetConnection(); nc.connect(null);

var ns:NetStream = new NetStream(nc); ns.addEventListener(AsyncErrorEvent.ASYNC_ERROR, asyncErrorHandler); ns.play(&quot;video.flv&quot;);

function asyncErrorHandler(event:AsyncErrorEvent):void

{

// ignore error

}

var vid:Video = new Video(); vid.attachNetStream(ns); addChild(vid);

pauseBtn.addEventListener(MouseEvent.CLICK, pauseHandler); playBtn.addEventListener(MouseEvent.CLICK, playHandler); stopBtn.addEventListener(MouseEvent.CLICK, stopHandler); togglePauseBtn.addEventListener(MouseEvent.CLICK, togglePauseHandler);

function pauseHandler(event:MouseEvent):void

{

ns.pause();

}

function playHandler(event:MouseEvent):void

{

ns.resume();

}

function stopHandler(event:MouseEvent):void

{

// Pause the stream and move the playhead back to

// the beginning of the stream. ns.pause();

ns.seek(0);

}

function togglePauseHandler(event:MouseEvent):void

{

ns.togglePause();

}

Clicking on the pauseBtn button instance while the video is playing causes the video file to pause. If the video is already paused, clicking this button has no effect. Clicking on the playBtn button instance resumes video playback if playback was previously paused, otherwise the button has no effect if the video was already playing.

**Detecting the end of a video stream**

In order to listen for the beginning and end of a video stream, you need to add an event listener to the NetStream instance to listen for the netStatus event. The following code demonstrates how to listen for the various codes throughout the video’s playback:

ns.addEventListener(NetStatusEvent.NET_STATUS, statusHandler); function statusHandler(event:NetStatusEvent):void

{

trace(event.info.code)

}

The previous code generates the following output:

NetStream.Play.Start NetStream.Buffer.Empty NetStream.Buffer.Full NetStream.Buffer.Empty NetStream.Buffer.Full NetStream.Buffer.Empty NetStream.Buffer.Full NetStream.Buffer.Flush NetStream.Play.Stop NetStream.Buffer.Empty NetStream.Buffer.Flush

The two codes that you want to specifically listen for are “NetStream.Play.Start” and “NetStream.Play.Stop” which signal the beginning and end of the video’s playback. The following snippet uses a switch statement to filter these two codes and trace a message:

function statusHandler(event:NetStatusEvent):void

{

switch (event.info.code)

{

case &quot;NetStream.Play.Start&quot;:

trace(&quot;Start [&quot; + ns.time.toFixed(3) + &quot; seconds]&quot;); break;

case &quot;NetStream.Play.Stop&quot;:

trace(&quot;Stop [&quot; + ns.time.toFixed(3) + &quot; seconds]&quot;); break;

}

}

By listening for the netStatus event (NetStatusEvent.NET_STATUS), you can build a video player which loads the next video in a playlist once the current video has finished playing.