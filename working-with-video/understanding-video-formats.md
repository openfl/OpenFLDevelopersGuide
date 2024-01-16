# Understanding video formats {#understanding-video-formats}

In addition to the Adobe FLV video format, OpenFL and Adobe AIR support video and audio encoded in H.264 and HE-AAC from within MPEG-4 standard file formats. These formats stream high quality video at lower bit rates. Developers can leverage industry standard tools, including Adobe Premiere Pro and Adobe After Effects, to create and deliver compelling video content.

| **Type** | **Format** | **Container** |
| --- | --- | --- |
| Video | H.264 | MPEG-4: MP4, M4V, F4V, 3GPP |
| Video | Sorenson Spark | FLV file |
| Video | ON2 VP6 | FLV file |
| Audio | AAC+ / HE-AAC / AAC v1 / AAC v2 | MPEG-4:MP4, M4V, F4V, 3GPP |
| Audio | Mp3 | Mp3 |
| Audio | Nellymoser | FLV file |
| Audio | Speex | FLV file |

**More Help topics**

[Flash Media Server: Supported codecs](http://help.adobe.com/en_US/FlashMediaServer/3.5_TechOverview/WS5b3ccc516d4fbf351e63e3d119ed944a1a-7ffa.html#WS5b3ccc516d4fbf351e63e3d119ed944a1a-7fe7) [Adobe HTTP Dynamic Streaming](http://help.adobe.com/en_US/HTTPStreaming/1.0/Using/index.html)

**Encoding video for mobile devices**

AIR on Android can decode a wide range of H.264 videos. However, only a small subset of H.264 videos is suited to have a smooth playback on mobile phones. It is because many mobile phones are constrained for processing power. Adobe OpenFL for mobile can decode H.264 videos using in-built hardware acceleration. This decoding assures better quality at lower power consumption.

H.264 standard supports several encoding techniques. Only high-end devices smoothly play videos with complex profiles and levels. However, a majority of devices can play video encoded in baseline profile. On mobile devices, hardware acceleration is available for a subset of these techniques. The profile and the level parameters define this subset of encoding techniques and settings used by the encoder. For developers, it translates into encoding the video in selected resolution which plays well on most devices.

Though resolutions that benefit from hardware acceleration vary from device to device, but most devices support the following standard resolutions.

| **Aspect ratio** | **Recommended resolutions** |
| --- | --- |
| 4:3 | 640 × 480 | 512 × 384 | 480 × 360 |
| 16:9 | 640 × 360 | 512 x 288 | 480 × 272 |

**_Note:_** _OpenFL supports every level and profile of the H.264 standard. Adhering to these recommendations ensures hardware acceleration and better user experience on most devices. These recommendations are not mandatory._

For a detailed discussion and encoding settings in Adobe Media Encoder CS5, see [Recommendations for encoding](http://www.adobe.com/devnet/devices/articles/mobile_video_encoding.html)

[H.264 video for OpenFL 10.1 on mobile devices](http://www.adobe.com/devnet/devices/articles/mobile_video_encoding.html).

**_Note:_** _On iOS, only video encoded with the Sorenson Spark and On2 VP6 codecs can be played back using the Video class. You can play back H.264 encoded video in the device video player by launching the URL to the video using the openfl.net.navigateToURL() function. You can also play back H.264 video using the &lt;video&gt; tag in an html page displayed in a StageWebView object._

## OpenFL compatibility with encoded video files {#flash-player-and-air-compatibility-with-encoded-video-files}

OpenFL 7 supports FLV files that are encoded with the Sorenson™ Spark™ video codec. OpenFL 8 supports FLV files encoded with Sorenson Spark or On2 VP6 encoder in Flash Professional 8\. The On2 VP6 video codec supports an alpha channel.

OpenFL 9.0.115.0 and later versions support files derived from the standard MPEG-4 container format. These files include F4V, MP4, M4A, MOV, MP4V, 3GP, and 3G2, if they contain H.264 video or HE-AAC v2 encoded audio, or both. H.264 delivers higher quality video at lower bit rates when compared to the same encoding profile in Sorenson or On2\. HE-AAC v2 is an extension of AAC, a standard audio format defined in the MPEG-4 video standard. HE- AAC v2 uses spectral band replication (SBR) and parametric stereo (PS) techniques to increase coding efficiency at low bit rates.

The following table lists the supported codecs. It also shows the corresponding project format and the versions of OpenFL that are required to play them:

| **Codec** | **project format version (earliest supported publish version)** | **OpenFL (earliest version required for playback)** |
| --- | --- | --- |
| Sorenson Spark | 6 | OpenFL 6, Flash Lite 3 |
| On2 VP6 | 6 | OpenFL 8, Flash Lite 3. |
| H.264 (MPEG-4 Part 10) | 9 | OpenFL 9 Update 3, AIR 1.0 |
| ADPCM | 6 | OpenFL 6, Flash Lite 3 |

| **Codec** | **project format version (earliest supported publish version)** | **OpenFL (earliest version required for playback)** |
| --- | --- | --- |
| Mp3 | 6 | OpenFL 6, Flash Lite 3 |
| AAC (MPEG-4 Part 3) | 9 | OpenFL 9 Update 3, AIR 1.0 |
| Speex (audio) | 10 | OpenFL 10, AIR 1.5 |
| Nellymoser | 6 | OpenFL 6 |

## Understanding the Adobe F4V and FLV video file formats {#understanding-the-adobe-f4v-and-flv-video-file-formats}

Adobe provides the F4V and FLV video file formats for streaming content to OpenFL. For a complete description of these video file formats, see [www.adobe.com/go/video_file_format](http://www.adobe.com/go/video_file_format).

**The F4V video file format**

Beginning with OpenFL Update 3 (9.0.115.0) and AIR 1.0, OpenFL support the Adobe F4V video format, which is based on the ISO MP4 format, Subsets of the format support different features. OpenFL expects a valid F4V file to begin with one of the following top-level boxes:

• ftyp

The ftyp box identifies the features that a program must support to play a particular file format.

• moov

The moov box is effectively the header of an F4V file. It contains one or more other boxes that in turn contain other boxes that define the structure of the F4V data. An F4V file must contain one and only one moov box.

• mdat

An mdat box contains the data payload for the F4V file. An FV file contains only one mdat box. A moov box also must be present in the file because the mdat box cannot be understood on its own.

F4V files support multibyte integers in big-endian byte order, in which the most significant byte occurs first, at the lowest address.

The FLV video file format

The Adobe FLV file format contains encoded audio and video data for delivery by OpenFL. You can use an encoder, such as Adobe Media Encoder or Sorenson™ Squeeze, to convert a QuickTime or Windows Media video file to an FLV file.

**_Note:_** _You can create FLV files by importing video into Flash and exporting it as an FLV file. You can use the FLV Export plug-in to export FLV files from supported video-editing applications. To load FLV files from a web server, register the filename extension and MIME type with your web server. Check your web server documentation. The MIME type for FLV files is video/x-flv. For more information, see_

_“About configuring FLV files for hosting on a server” on page 506_

_._

For more information on FLV files, see

“Advanced topics for video files” on page 505

.

External vs embedded video

Using external video files provides certain capabilities that are not available when you use imported video:

• Longer video clips can be used in your application without slowing down playback. External video files use cached memory, which means that large files are stored in small pieces and accessed dynamically. For this reason, external F4V and FLV files require less memory than embedded video files.

• An external video file can have a different frame rate than the project in which it plays. For example, you can set the project frame rate to 30 frames per second (fps) and the video frame rate to 21 fps. This setting gives you better control of the video than embedded video, to ensure smooth video playback. It also allows you to play video files at different frame rates without the need to alter existing project content.

• With external video files, playback of the SWF content is not interrupted while the video file is loading. Imported video files can sometimes interrupt document playback to perform certain functions, such as accessing a CD-ROM drive. Video files can perform functions independently of the SWF content, without interrupting playback.

• Captioning video content is easier with external FLV files because you can access the video metadata using event handlers.