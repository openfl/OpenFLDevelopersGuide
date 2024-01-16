# Chapter 24: Working with sound {#chapter-24-working-with-sound}

Haxe is made for immersive, interactive applications—and one often overlooked element of powerfully immersive applications is sound. You can add sound effects to a video game, audio feedback to an application user interface, or even make a program that analyzes mp3 files loaded over the Internet, with sound at the core of the application.

You can load external audio files and work with audio that’s embedded in a SWF. You can control the audio, create visual representations of the sound information, and capture sound from a user’s microphone.

**More Help topics**

[openfl.media package](https://api.openfl.org/openfl/media/index.html)
[openfl.events.SampleDataEvent](https://api.openfl.org/openfl/events/SampleDataEvent.html)

**Basics of working with sound**

Computers can capture and encode digital audio—computer representation of sound information—and can store it and retrieve it to play back over speakers. You can play back sound using either Adobe® Flash® Player or Adobe® AIR™ and Haxe.

When sound data is converted to digital form, it has various characteristics, such as the sound’s volume and whether it is stereo or mono sound. When you play back a sound in Haxe, you can adjust these characteristics as well— make the sound louder, or make it seem to be coming from a certain direction, for instance.

Before you can control a sound in Haxe, you need to have the sound information loaded into OpenFL. There are five ways you can get audio data into OpenFL so that you can work with it using Haxe.

*   Load an external sound file such as an mp3 file into the SWF.
*   Embed the sound information into the project directly when it’s being created.
*   Capture audio from a microphone attached to a user’s computer.
*   Stream audio from a server.
*   Dynamically generate and play audio.

When you load sound data from an external sound file, you can begin playing back the start of the sound file while the rest of the sound data is still loading.

Although there are various sound file formats used to encode digital audio, Haxe, OpenFL support sound files that are stored in the mp3 format. They cannot directly load or play sound files in other formats like WAV or AIFF.

While you’re working with sound in Haxe, you’ll likely work with several classes from the openfl.media package. The Sound class is the class you use to get access to audio information by loading a sound file or assigning a function to an event that samples sound data and then starting playback. Once you start playing a sound, OpenFL give you access to a SoundChannel object. Since an audio file that you’ve loaded may only be one of several sounds that you play on a user’s computer, each individual sound that’s playing uses its own SoundChannel object; the combined output of all the SoundChannel objects mixed together is what actually plays over the computer’s speakers. You use this SoundChannel instance to control properties of the sound and to stop its playback. Finally, if you want to control the combined audio, the SoundMixer class gives you control over the mixed output.

You can also use several other classes to perform more specific tasks when you’re working with sound in Haxe; for more information on all the sound-related classes, see

“Understanding the sound architecture” on page 442

.

Important concepts and terms

The following reference list contains important terms that you may encounter:

**Amplitude** The distance of a point on the sound waveform from the zero or equilibrium line.

**Bit rate** The amount of data that is encoded or streamed for each second of a sound file. For mp3 files, the bit rate is usually stated in terms of thousands of bits per second (kbps). A higher bit rate generally means a higher quality sound wave.

**Buffering** The receiving and storing of sound data before it is played back.

**mp3** MPEG-1 Audio Layer 3, or mp3, is a popular sound compression format.

**Panning** The positioning of an audio signal between the left and right channels in a stereo soundfield.

**Peak** The highest point in a waveform.

**Sampling rate** Defines the number of samples per second taken from an analog audio signal to make a digital signal. The sampling rate of standard compact disc audio is 44.1 kHz or 44,100 samples per second.

**Streaming** The process of playing the early portions of a sound file or video file while later portions of that file are still being loaded from a server.

**Volume** The loudness of a sound.

**Waveform** The shape of a graph of the varying amplitudes of a sound signal over time.