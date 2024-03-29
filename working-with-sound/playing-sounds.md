# Playing sounds {#playing-sounds}

Playing a loaded sound can be as simple as calling the `Sound.play()` method for
a [Sound object](https://api.openfl.org/openfl/media/Sound.html), as follows:

```haxe
var snd:Sound = new Sound(new URLRequest("smallSound.mp3"));
snd.play();
```

> _Warning:_ When targeting HTML and JavaScript, web browsers may ignore
> attempts to play sounds until after the user has interacted with the page—such
> as clicking, tapping, or pressing keys. See
> [MDN: Autoplay guide for media and Web Audio APIs](https://developer.mozilla.org/en-US/docs/Web/Media/Autoplay_guide)
> for more details.

When playing back sounds using OpenFL, you can perform the following operations:

- Play a sound from a specific starting position

- Pause a sound and resume playback from the same position later

- Know exactly when a sound finishes playing

- Track the playback progress of a sound

- Change volume or panning while a sound plays

To perform these operations during playback, use the SoundChannel and
SoundTransform classes.

The SoundChannel class controls the playback of a single sound. The
`SoundChannel.position` property can be thought of as a playhead, indicating the
current point in the sound data that's being played.

When an application calls the `Sound.play()` method, a new instance of the
SoundChannel class is created to control the playback.

Your application can play a sound from a specific starting position by passing
that position, in terms of milliseconds, as the `startTime` parameter of the
`Sound.play()` method. It can also specify a fixed number of times to repeat the
sound in rapid succession by passing a numeric value in the `loops` parameter of
the `Sound.play()` method.

When the `Sound.play()` method is called with both a `startTime` parameter and a
`loops` parameter, the sound is played back repeatedly from the same starting
point each time, as shown in the following code:

```haxe
var snd:Sound = new Sound(new URLRequest("repeatingSound.mp3"));
snd.play(1000, 3);
```

In this example, the sound is played from a point one second after the start of
the sound, three times in succession.

## Pausing and resuming a sound

If your application plays long sounds, like songs or podcasts, you probably want
to let users pause and resume the playback of those sounds. A sound cannot
literally be paused during playback in OpenFL; it can only be stopped. However,
a sound can be played starting from any point. You can record the position of
the sound at the time it was stopped, and then replay the sound starting at that
position later.

For example, let's say your code loads and plays a sound file like this:

```haxe
var snd:Sound = new Sound(new URLRequest("bigSound.mp3"));
var channel:SoundChannel = snd.play();
```

While the sound plays, the `SoundChannel.position` property indicates the point
in the sound file that is currently being played. Your application can store the
position value before stopping the sound from playing, as follows:

```haxe
var pausePosition = channel.position;
channel.stop();
```

To resume playing the sound, pass the previously stored position value to
restart the sound from the same point it stopped at before.

```haxe
channel = snd.play(pausePosition);
```

## Monitoring playback

Your application might want to know when a sound stops playing so it can start
playing another sound, or clean up some resources used during the previous
playback. The SoundChannel class dispatches an `Event.SOUND_COMPLETE` event when
its sound finishes playing. Your application can listen for this event and take
appropriate action, as shown below:

```haxe
import openfl.display.Sprite;
import openfl.events.Event;
import openfl.media.Sound;
import openfl.media.SoundChannel;
import openfl.net.URLRequest;

class SoundCompleteEventExample extends Sprite {
	public function new() {
		super();

		var snd:Sound = new Sound();
		var req:URLRequest = new URLRequest("smallSound.mp3");
		snd.load(req);

		var channel:SoundChannel = snd.play();
		channel.addEventListener(Event.SOUND_COMPLETE, onPlaybackComplete);
	}

	function onPlaybackComplete(event:Event):Void {
		trace("The sound has finished playing.");
	}
}
```

The SoundChannel class does not dispatch progress events during playback. To
report on playback progress, your application can set up its own timing
mechanism and track the position of the sound playhead.

To calculate what percentage of a sound has been played, you can divide the
value of the `SoundChannel.position` property by the length of the sound data
that's being played:

```haxe
var playbackPercent = 100.0 * (channel.position / snd.length);
```

However, this code only reports accurate playback percentages if the sound data
was fully loaded before playback began. The `Sound.length` property shows the
size of the sound data that is currently loaded, not the eventual size of the
entire sound file. To track the playback progress of a streaming sound that is
still loading, your application should estimate the eventual size of the full
sound file and use that value in its calculations. You can estimate the eventual
length of the sound data using the `bytesLoaded` and `bytesTotal` properties of
the Sound object, as follows:

```haxe
var estimatedLength = Math.ceil(snd.length / (snd.bytesLoaded / snd.bytesTotal));
var playbackPercent = 100.0 * (channel.position / estimatedLength);
```

The following code loads a larger sound file and uses the `Event.ENTER_FRAME`
event as its timing mechanism for showing playback progress. It periodically
reports on the playback percentage, which is calculated as the current position
value divided by the total length of the sound data:

```haxe
import openfl.media.SoundChannel;
import openfl.display.Sprite;
import openfl.events.Event;
import openfl.media.Sound;
import openfl.net.URLRequest;

class SoundProgressEventExample extends Sprite {
	private var snd:Sound;
	private var channel:SoundChannel;

	public function new() {
		super();

		snd = new Sound();
		var req:URLRequest = new URLRequest("bigSound.mp3");
		snd.load(req);

		channel = snd.play();
		addEventListener(Event.ENTER_FRAME, onEnterFrame);
		channel.addEventListener(Event.SOUND_COMPLETE, onPlaybackComplete);
	}

	function onEnterFrame(event:Event):Void {
		var estimatedLength:Int = Math.ceil(snd.length / (snd.bytesLoaded / snd.bytesTotal));
		var playbackPercent:Int = Math.round(100 * (channel.position / estimatedLength));
		trace("Sound playback is " + playbackPercent + "% complete.");
	}

	function onPlaybackComplete(event:Event):Void {
		trace("The sound has finished playing.");
		removeEventListener(Event.ENTER_FRAME, onEnterFrame);
	}
}
```

After the sound data starts loading, this code calls the `snd.play()` method and
stores the resulting SoundChannel object in the `channel` variable. Then it adds
an event listener to the main application for the `Event.ENTER_FRAME` event and
another event listener to the SoundChannel object for the `Event.SOUND_COMPLETE`
event that occurs when playback is complete.

Each time the application reaches a new frame in its animation, the
`onEnterFrame()` method is called. The `onEnterFrame()` method estimates the
total length of the sound file based on the amount of data that has already been
loaded, and then it calculates and displays the current playback percentage.

When the entire sound has been played, the `onPlaybackComplete()` method
executes, removing the event listener for the `Event.ENTER_FRAME` event so that
it doesn't try to display progress updates after playback is done.

The `Event.ENTER_FRAME` event can be dispatched many times per second. In some
cases, you won't want to display playback progress that frequently. In those
cases, your application can set up its own timing mechanism using the
[openfl.utils.Timer class](https://api.openfl.org/openfl/utils/Timer.html).

## Stopping streaming sounds

There is a quirk in the playback process for sounds that are streaming—that is,
for sounds that are still loading while they are being played. When your
application calls the `SoundChannel.stop()` method on a SoundChannel instance
that is playing back a streaming sound, the sound playback stops for one frame,
and then on the next frame, it restarts from the beginning of the sound. This
occurs because the sound loading process is still underway. To stop both the
loading and the playback of a streaming sound, call the `Sound.close()` method.
