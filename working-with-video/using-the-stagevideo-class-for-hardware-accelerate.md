# Using the StageVideo class for hardware accelerated presentation {#using-the-stagevideo-class-for-hardware-accelerated-presentation}

OpenFL 10.2 and later

OpenFL optimizes video performance by using hardware acceleration for H.264 decoding. You can enhance performance further by using the StageVideo API. Stage video lets your application take advantage of hardware accelerated presentation.

Runtimes that support the StageVideo API include:

• OpenFL 10.2 and later

**_Note:_ **_In OpenFL 11.4/AIR 3.4 and higher, you can use camera input with StageVideo._

Downloadable source code and additional details for the stage video feature are available at [Getting Started with](http://www.adobe.com/go/learn_as3_usingstagevideo_en) [Stage Video](http://www.adobe.com/go/learn_as3_usingstagevideo_en).

For a StageVideo quick start tutorial, see [Working with Stage Video](http://www.adobe.com/go/learn_as3_workingwithstagevideo_en).

**About hardware acceleration using StageVideo**

Hardware accelerated presentation—which includes video scaling, color conversion, and blitting—enhances the performance benefits of hardware accelerated decoding. On devices that offer GPU (hardware) acceleration, you can use a flash.media.StageVideo object to process video directly on the device hardware. Direct processing frees the CPU to perform other tasks while the GPU handles video. The legacy Video class, on the other hand, typically uses software presentation. Software presentation occurs in the CPU and can consume a significant share of system resources.

Currently, few devices provide full GPU acceleration. However, stage video lets applications take maximum advantage of whatever hardware acceleration is available.

The StageVideo class does not make the Video class obsolete. Working together, these two classes provide the optimal video display experience allowed by device resources at any given time. Your application takes advantage of hardware acceleration by listening to the appropriate events and switching between StageVideo and Video as necessary.

The StageVideo class imposes certain restrictions on video usage. Before implementing StageVideo, review the guidelines and make sure your application can accept them. If you accept the restrictions, use the StageVideo class whenever OpenFL detects that hardware accelerated presentation is available. See

“Guidelines and limitations” on

page 514

.

Parallel planes: Stage video and the Flash display list

With the stage video model, OpenFL can separate video from the display list. OpenFL divides the composite display between two Z-ordered planes:

**Stage video plane** The stage video plane sits in the background. It displays only hardware accelerated video. Because of this design, this plane is not available if hardware acceleration is not supported or not available on the device. In Haxe, StageVideo objects handle videos played on the stage video plane.

**Flash display list plane** Flash display list entities are composited on a plane in front of the stage video plane. Display list entities include anything that the runtime renders, including playback controls. When hardware acceleration is not available, videos can be played only on this plane, using the Video class object. Stage video always displays behind Flash display list graphics.

_Video display planes_

The StageVideo object appears in a non-rotated, window-aligned rectangular region of the screen. You cannot layer objects behind the stage video plane. However, you can use the Flash display list plane to layer other graphics on top of the stage video plane. Stage video runs concurrently with the display list. Thus, you can use the two mechanisms together to create a unified visual effect that uses two discreet planes. For example, you can use the front plane for playback controls that operate on the stage video running in the background.

Stage video and H.264 codec

In OpenFL applications, implementing video hardware acceleration involves two steps:

1.  Encoding the video as H.264
2.  Implementing the StageVideo API

For best results, perform both steps. The H.264 codec lets you take maximum advantage of hardware acceleration, from video decoding to presentation.

Stage video eliminates GPU-to-CPU read-back. In other words, the GPU no longer sends decoded frames back to the CPU for compositing with display list objects. Instead, the GPU blits decoded and rendered frames directly to the screen, behind the display list objects. This technique reduces CPU and memory usage and also provides better pixel fidelity.

More Help topics

“Understanding video formats” on page 475

Guidelines and limitations

When video is running in full screen mode, stage video is always available if the device supports hardware acceleration. OpenFL, however, also runs within a browser. In the browser context, the wmode setting affects stage video availability. Try to use wmode=&quot;direct&quot; at all times if you want to use stage video. Stage video is not compatible with other wmode settings when not in full screen mode. This restriction means that, at run time, stage video can vacillate unpredictably between being available and unavailable. For example, if the user exits full screen mode while stage video is running, the video context reverts to the browser. If the browser wmode parameter is not set to &quot;direct&quot;, stage video can suddenly become unavailable. OpenFL communicates playback context changes to applications through a set of events. If you implement the StageVideo API, maintain a Video object as a backup when stage video becomes unavailable.

Because of its direct relationship to hardware, stage video restricts some video features. Stage video enforces the following constraints:

• For each project, OpenFL limits the number of StageVideo objects that can concurrently display videos to four. However, the actual limit can be lower, depending on device hardware resources.

• The video timing is not synchronized with the timing of content that the runtime displays.

• The video display area can only be a rectangle. You cannot use more advanced display areas, such as elliptical or irregular shapes.

• You cannot rotate the video.

• You cannot bitmap cache the video or use BitmapData object to access it.

• You cannot apply filters to the video.

• You cannot apply color transforms to the video.

• You cannot apply an alpha value to the video.

• Blend modes that you apply to objects in the display list plane do not apply to stage video.

• You can place the video only on full pixel boundaries.

• Though GPU rendering is the best available for the given device hardware, it is not 100% “pixel identical” across devices. Slight variations occur due to driver and platform differences.

• A few devices do not support all required color spaces. For example, some devices do not support BT.709, the H.264 standard. In such cases, you can use BT.601 for fast display.

• You cannot use stage video with WMODE settings such as normal, opaque, or transparent. Stage video supports only WMODE=direct when not in full screen mode. WMODE has no effect in Safari 4 or higher and IE 9 or higher.

In most cases, these limitations do not affect video player applications. If you can accept these limitations, use stage video whenever possible.

More Help topics

“Working with full-screen mode” on page 167

## Using the StageVideo APIs {#using-the-stagevideo-apis}

Stage video is a mechanism within the runtime that enhances video playback and device performance. The runtime creates and maintains this mechanism; as a developer, your role is to configure your application to take advantage of it.

To use stage video, you implement a framework of event handlers that detect when stage video is and isn’t available. When you receive notification that stage video is available, you retrieve a StageVideo object from the Stage.stageVideos property. The runtime populates this Vector object with one or more StageVideo objects. You can then use one of the provided StageVideo objects, rather than a Video object, to display streaming video.

On OpenFL, when you receive notification that stage video is no longer available, switch your video stream back to a Video object.

**_Note:_ **_You cannot create StageVideo objects._

Stage.stageVideos property

The Stage.stageVideos property is a Vector object that gives you access to StageVideo instances. This vector can contain up to four StageVideo objects, depending on hardware and system resources. Mobile devices can be limited to one, or none.

When stage video is not available, this vector contains no objects. To avoid run time errors, be sure that you access members of this vector only when the most recent StageVideoAvailability event indicates that stage video is available.

StageVideo events

The StageVideo API framework provides the following events:

**StageVideoAvailabilityEvent.STAGE_VIDEO_AVAILABILITY** Sent when the Stage.stageVideos property changes. The StageVideoAvailabilityEvent.availability property indicates either AVAILABLE or UNAVAILABLE. Use this event to determine whether the stageVideos property contains any StageVideo objects, rather than directly checking the length of the Stage.stageVideos vector.

**StageVideoEvent.RENDER_STATE** Sent when a NetStream or Camera object has been attached to a StageVideo object and is playing. Indicates the type of decoding currently in use: hardware, software, or unavailable (nothing is displayed). The event target contains videoWidth and videoHeight properties that are safe to use for resizing the video viewport.

**_Important:_ **_Coordinates obtained from the StageVideo target object use Stage coordinates, since they are not part of the standard display list._

**VideoEvent.RENDER_STATE** Sent when a Video object is being used. Indicates whether software or hardware accelerated decoding is in use. If this event indicates hardware accelerated decoding, switch to a StageVideo object if possible. The Video event target contains videoWidth and videoHeight properties that are safe to use for resizing the video viewport.

Workflow for implementing the StageVideo feature

Follow these top-level steps to implement the StageVideo feature:

1.  Listen for the StageVideoAvailabilityEvent.STAGE_VIDEO_AVAILABILITY event to find out when the Stage.stageVideos vector has changed. See

    “Using the

    StageVideoAvailabilityEvent.STAGE_VIDEO_AVAILABILITY event” on page 517

    .
2.  If the StageVideoAvailabilityEvent.STAGE_VIDEO_AVAILABILITY event reports that stage video is available, use the Stage.stageVideos Vector object within the event handler to access a StageVideo object.
3.  Attach a NetStream object using StageVideo.attachNetStream() or attach a Camera object using

StageVideo.attachCamera().

1.  Play the video using NetStream.play().
2.  Listen for the StageVideoEvent.RENDER_STATE event on the StageVideo object to determine the status of playing the video. Receipt of this event also indicates that the width and height properties of the video have been initialized or changed. See

    “Using the StageVideoEvent.RENDER_STATE and VideoEvent.RENDER_STATE events” on

    page 519

    .
3.  Listen for the VideoEvent.RENDER_STATE event on the Video object. This event provides the same statuses as StageVideoEvent.RENDER_STATE, so you can also use it to determine whether GPU acceleration is available. Receipt of this event also indicates that the width and height properties of the video have been initialized or changed. See

    “Using the StageVideoEvent.RENDER_STATE and VideoEvent.RENDER_STATE events” on

    page 519

    .

Initializing StageVideo event listeners

Set up your StageVideoAvailabilityEvent and VideoEvent listeners during application initialization. For example, you can initialize these listeners in the flash.events.Event.ADDED_TO_STAGE event handler. This event guarantees that your application is visible on the stage:

public class SimpleStageVideo extends Sprite private var nc:NetConnection;

private var ns:NetStream;

public function SimpleStageVideo()

{

// Constructor for SimpleStageVideo class

// Make sure the app is visible and stage available addEventListener(Event.ADDED_TO_STAGE, onAddedToStage);

}

private function onAddedToStage(event:Event):void

{

//...

// Connections

nc = new NetConnection(); nc.connect(null);

ns = new NetStream(nc); ns.addEventListener(NetStatusEvent.NET_STATUS, onNetStatus); ns.client = this;

// Screen

video = new Video(); video.smoothing = true;

// Video Events

// the StageVideoEvent.STAGE_VIDEO_STATE informs you whether

// StageVideo is available stage.addEventListener(StageVideoAvailabilityEvent.STAGE_VIDEO_AVAILABILITY,

onStageVideoState);

// in case of fallback to Video, listen to the VideoEvent.RENDER_STATE

// event to handle resize properly and know about the acceleration mode running video.addEventListener(VideoEvent.RENDER_STATE, videoStateChange);

//...

}

Using the StageVideoAvailabilityEvent.STAGE_VIDEO_AVAILABILITY event

In the StageVideoAvailabilityEvent.STAGE_VIDEO_AVAILABILITY handler, decide whether to use a Video or StageVideo object based on the availability of StageVideo. If the StageVideoAvailabilityEvent.availability property is set to StageVideoAvailability.AVAILABLE, use StageVideo. In this case, you can rely on the Stage.stageVideos vector to contain one or more StageVideo objects. Obtain a StageVideo object from the Stage.stageVideos property and attach the NetStream object to it. Because StageVideo objects always appear in the background, remove any existing Video object (always in the foreground). You also use this event handler to add a listener for the StageVideoEvent.RENDER_STATE event.

If the StageVideoAvailabilityEvent.availability property is set to StageVideoAvailability.UNAVAILABLE, do not use StageVideo or access the Stage.stageVideos vector. In this case, attach the NetStream object to a Video object. Finally, add the StageVideo or Video object to the stage and call NetStream.play().

The following code shows how to handle the StageVideoAvailabilityEvent.STAGE_VIDEO_AVAILABILITY event:

private var sv:StageVideo; private var video:Video;

private function onStageVideoState(event:StageVideoAvailabilityEvent):void

{

// Detect if StageVideo is available and decide what to do in toggleStageVideo toggleStageVideo(event.availability == StageVideoAvailability.AVAILABLE);

}

private function toggleStageVideo(on:Boolean):void

{

// To choose StageVideo attach the NetStream to StageVideo if (on)

{

stageVideoInUse = true; if ( sv == null )

{

sv = stage.stageVideos[0]; sv.addEventListener(StageVideoEvent.RENDER_STATE, stageVideoStateChange);

sv.attachNetStream(ns);

}

if (classicVideoInUse)

{

}

} else

{

// If you use StageVideo, remove from the display list the

// Video object to avoid covering the StageVideo object

// (which is always in the background) stage.removeChild ( video ); classicVideoInUse = false;

// Otherwise attach it to a Video object if (stageVideoInUse)

stageVideoInUse = false; classicVideoInUse = true; video.attachNetStream(ns); stage.addChildAt(video, 0);

}

if ( !played )

{

played = true; ns.play(FILE_NAME);

}

}

**_Important:_ **_The first time an application accesses the vector element at Stage.stageVideos[0], the default rect is set to 0,0,0,0, and pan and zoom properties use default values. Always reset these values to your preferred settings. You can use the videoWidth and videoHeight properties of the StageVideoEvent.RENDER_STATE or VideoEvent.RENDER_STATE event target for calculating the video viewport dimensions._

Download the full source code for this sample application at [Getting Started with Stage Video](http://www.adobe.com/go/learn_as3_usingstagevideo_en).

**Using the StageVideoEvent.RENDER_STATE and VideoEvent.RENDER_STATE events** StageVideo and Video objects send events that inform applications when the display environment changes. These events are StageVideoEvent.RENDER_STATE and VideoEvent.RENDER_STATE.

A StageVideo or Video object dispatches a render state event when a NetStream object is attached and begins playing. It also sends this event when the display environment changes; for example, when the video viewport is resized. Use these notifications to reset your viewport to the current videoHeight and videoWidth values of the event target object.

Reported render states include:

• RENDER_STATUS_UNAVAILABLE

• RENDER_STATUS_SOFTWARE

• RENDER_STATUS_ACCELERATED

Render states indicate when hardware accelerated decoding is in use, regardless of which class is currently playing video. Check the StageVideoEvent.status property to learn whether the necessary decoding is available. If this property is set to “unavailable”, the StageVideo object cannot play the video. This status requires that you immediately reattach the NetStream object to a Video object. Other statuses inform your application of the current rendering conditions.

The following table describes the implications of all render status values for StageVideoEvent and VideoEvent objects in OpenFL:

|  | **VideoStatus.ACCELERATED** | **VideoStatus.SOFTWARE** | **VideoStatus.UNAVAILABLE** |
| --- | --- | --- | --- |
| StageVideoEvent | Decoding and presentation both occur in hardware. (Best performance.) | Presentation in hardware, decoding in software. (Acceptable performance.) | No GPU resources are available to handle video, and nothing is displayed. **Fall back to a Video object.** |
| VideoEvent | Presentation in software, decoding in hardware. (Acceptable performance on a modern desktop system only. Degraded full-screen performance.) | Presentation in software, decoding in software. (Worst case performance- wise. Degraded full-screen performance.) | (N/A) |

Color spaces

Stage video uses underlying hardware capabilities to support color spaces. SWF content can provide metadata indicating its preferred color space. However, the device graphics hardware determines whether that color space can be used. One device can support several color spaces, while another supports none. If the hardware does not support the requested color space, OpenFL attempts to find the closest match among the supported color spaces.

To query which color spaces the hardware supports, use the StageVideo.colorSpaces property. This property returns the list of supported color spaces in a String vector:

var colorSpace:Vector.&lt;String&gt; = stageVideo.colorSpaces();

To learn which color space the currently playing video is using, check the StageVideoEvent.colorSpace property. Check this property in your event handler for the StageVideoEvent.RENDER_STATE event:

var currColorSpace:String;

//StageVideoEvent.RENDER_STATE event handler

private function stageVideoRenderState(event:Object):void

{

//...

currColorSpace = (event as StageVideoEvent).colorSpace;

//...

}

If OpenFL cannot find a substitute for an unsupported color space, stage video uses the default color space BT.601\. For example, video streams with H.264 encoding typically use the BT.709 color space. If the device hardware does not support BT.709, the colorSpace property returns &quot;BT601&quot;. A StageVideoEvent.colorSpace value of &quot;unknown&quot; indicates that the hardware does not provide a means of querying the color space.

If your application deems the current color space unacceptable, you can choose to switch from a StageVideo object to a Video object. The Video class supports all color spaces through software compositing.