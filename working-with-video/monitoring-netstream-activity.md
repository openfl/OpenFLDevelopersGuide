# Monitoring NetStream activity {#monitoring-netstream-activity}

OpenFL 10.3 and later, Adobe AIR 2.7 and later

You can monitor NetStream activity to collect the information required to support media usage analysis and reporting. The monitoring features discussed in this section allow you to create media measurement libraries that collect data without close coupling to the particular video player displaying the media. This allows your client developers to choose their favorite video players when using your library. Use the NetMonitor class to monitor the creation and activity of NetStream objects in an application. The NetMonitor class provides a list of the active NetStreams existing at any given time and also dispatches an event whenever a NetStream object is created.

A NetStream object dispatches the events listed in the following table, depending on the type of media being played:

| **Event** | **Progressive download** | **RTMP streaming** | **HTTP streaming** |
| --- | --- | --- | --- |
| NetStream.Play.Start | Yes | Yes | No |
| NetStream.Play.Stop | Yes | Yes | No |
| NetStream.Play.Complete | Yes | Yes | No |
| NetStream.SeekStart.Notify | Yes | Yes | Yes |
| NetStream.Seek.Notify | Yes | Yes | Yes |
| NetStream.Unpause.Notify | Yes | Yes | Yes |
| NetStream.Unpause.Notify | Yes | Yes | Yes |
| NetStream.Play.Transition | Not applicable | Yes | Not applicable |
| NetStream.Play.TransitionComplete | Not applicable | Yes | Not applicable |
| NetStream.Buffer.Full | Yes | Yes | Yes |
| NetStream.Buffer.Flush | Yes | Yes | Yes |
| NetStream.Buffer.Empty | Yes | Yes | Yes |

The NetStreamInfo object associated with a NetStream instance also stores the last metadata and XMP data objects that were encountered in the media.

When media is played via HTTP streaming, the NetStream.Play.Start, NetStream.Play.Stop, and NetStream.Play.Complete are not dispatched since the application has complete control of the media stream. A video player should synthesize and dispatch these events for HTTP streams.

Likewise, NetStream.Play.Transition and NetStream.Play.TransitionComplete are not dispatched for either progressive download or HTTP media. Dynamic bitrate switching is an RTMP feature. If a video player using an HTTP stream supports a similar feature, the player can synthesize and dispatch transition events.

**More Help topics**

[Adobe Developer Connection: Measuring video consumption in Flash](http://www.adobe.com/devnet/video/articles/media-measurement-flash.html)

**Monitoring NetStream events**

Two types of events provide valuable usage data: netStatus and mediaTypeData. In addition, a timer can be used to periodically log the position of the NetStream playhead.

netStatus events provide information you can use to determine how much of a stream a user viewed. Buffer and RTMFP stream transition events also result in a netStatus event.

mediaTypeData events provide meta and XMP data information. The Netstream.Play.Complete event is dispatched as a mediaTypeData event. Other data embedded in the stream are also available through mediaTypeData events, including cue points, text, and images.

The following example illustrates how to create a class that monitors status and data events from any active NetStreams in an application. Typically, such a class would upload the data it was interested in analyzing to a server for collection.

package com.adobe.example

{

import openfl.events.NetDataEvent;
import openfl.events.NetMonitorEvent;
import openfl.events.NetStatusEvent;
import openfl.net.NetMonitor;

import openfl.net.NetStream;

public class NetStreamEventMonitor

{

private var netmon:NetMonitor;

private var heartbeat:Timer = new Timer( 5000 );

public function NetStreamEventMonitor()

{

//Create NetMonitor object netmon = new NetMonitor();

netmon.addEventListener( NetMonitorEvent.NET_STREAM_CREATE, newNetStream );

//Start the heartbeat timer

heartbeat.addEventListener( TimerEvent.TIMER, onHeartbeat ); heartbeat.start();

}

//On new NetStream

private function newNetStream( event:NetMonitorEvent ):void

{

trace( "New Netstream object");

var stream:NetStream = event.netStream; stream.addEventListener(NetDataEvent.MEDIA_TYPE_DATA, onStreamData); stream.addEventListener(NetStatusEvent.NET_STATUS, onStatus);

}

//On data events from a NetStream object

private function onStreamData( event:NetDataEvent ):void

{

var netStream:NetStream = event.target as NetStream;

trace( "Data event from " + netStream.info.uri + " at " + event.timestamp ); switch( event.info.handler )

{

case "onMetaData":

//handle metadata; break;

case "onXMPData":

//handle XMP; break;

case "onPlayStatus":

//handle NetStream.Play.Complete case "onImageData":

//handle image break;

case "onTextData":

//handle text break;

default:

//handle other events

}

}

//On status events from a NetStream object

private function onStatus( event:NetStatusEvent ):void

{

trace( "Status event from " + event.target.info.uri + " at " + event.target.time );

//handle status events

}

//On heartbeat timer

private function onHeartbeat( event:TimerEvent ):void

{

var streams:Vector.&lt;NetStream&gt; = netmon.listStreams(); for( var i:int = 0; i &lt; streams.length; i++ )

{

trace( "Heartbeat on " + streams[i].info.uri + " at " + streams[i].time );

//handle heartbeat event

}

}

}

}

## Detecting player domain {#detecting-player-domain}

The URL and domain of the web page on which a user is viewing media content is not always readily available. If allowed by the hosting web site, you can use the ExternalInterface class to get the exact URL. However, some web sites that allow third-party video players do not allow the ExternalInterface to be used. In such cases, you can get the domain of the current web page from the pageDomain property of the Security class. The full URL is not divulged for user security and privacy reasons.

The page domain is available from the static pageDomain property of the Security class:

var domain:String = Security.pageDomain;