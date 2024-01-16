# Understanding the sound architecture {#understanding-the-sound-architecture}

Your applications can load sound data from five main sources:

- External sound files loaded at run time

- Sound resources embedded as assets within the application

<!-- TODO: uncomment when microphone is implemented
- Sound data from a microphone attached to the user's system-->

<!-- TODO: uncomment if FMS streaming is implemented
- Sound data streamed from a remote media server, such as Flash Media Server -->

- Sound data being generated dynamically through the use of the `sampleData`
  event handler

Sound data can be fully loaded before it is played back, or it can be streamed,
meaning that it is played back while it is still loading.

The OpenFL sound classes support sound files that are stored in the mp3, ogg, or
wav formats. They cannot directly load or play sound files in other formats,
such as AAC or AIFF.

The OpenFL sound architecture makes use of the following classes in the
[openfl.media package](https://api.openfl.org/openfl/media/index.html).

| Class                                                                                          | Description                                                                                                                                                                                                                                                           |
| ---------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [openfl.media.Sound](https://api.openfl.org/openfl/media/Sound.html)                           | The Sound class handles the loading of sound, manages basic sound properties, and starts a sound playing.                                                                                                                                                             |
| [openfl.media.SoundChannel](https://api.openfl.org/openfl/media/SoundChannel.html)             | When an application plays a Sound object, a new SoundChannel object is created to control the playback. The SoundChannel object controls the volume of both the left and right playback channels of the sound. Each sound that plays has its own SoundChannel object. |
| [openfl.media.SoundLoaderContext](https://api.openfl.org/openfl/media/SoundLoaderContext.html) | The SoundLoaderContext class specifies how many seconds of buffering to use when loading a sound, and whether OpenFL looks for a policy file from the server when loading a file. A SoundLoaderContext object is used as a parameter to the `Sound.load()` method.    |
| [openfl.media.SoundTransform](https://api.openfl.org/openfl/media/SoundTransform.html)         | The SoundTransform class contains values that control sound volume and panning. A SoundTransform object can be applied to an individual SoundChannel object, to the global SoundMixer object, or to a Microphone object, among others.                                |
| [openfl.media.ID3Info](https://api.openfl.org/openfl/media/ID3Info.html)                       | An ID3Info object contains properties that represent ID3 metadata information that is often stored in mp3 sound files.                                                                                                                                                |

<!-- TODO: uncomment if SoundMixer is implemented
| openfl.media.SoundMixer         | The SoundMixer class controls playback and security properties that pertain to all sounds in an application. In effect, multiple sound channels are mixed through a common SoundMixer object, so property values in the SoundMixer object will affect all SoundChannel objects that are currently playing. |-->
<!-- TODO: uncomment if microphone is implemented
| openfl.media.Microphone         | The Microphone class represents a microphone or other sound input device attached to the user's computer. Audio input from a microphone can be routed to local speakers or sent to a remote server. The Microphone object controls the gain, sampling rate, and other characteristics of its own sound stream. |-->

<!-- TODO: uncomment if SoundMixer is implemented
| openfl.media.AudioPlaybackMode  | The AudioPlaybackMode class defines constants for the `audioPlaybackMode` property of the SoundMixer class.                                                                                                                                                                                                    | -->

Each sound that is loaded and played needs its own instance of the Sound class
and the SoundChannel class.

<!-- TODO uncomment if SoundMixer and/or microphone is implemented
The output from multiple SoundChannel instances is
then mixed together by the global SoundMixer class during playback.

The Sound, SoundChannel, and SoundMixer classes are not used for sound data
obtained from a microphone or from a streaming media server like Flash Media
Server.-->
