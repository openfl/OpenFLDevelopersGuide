# Network connectivity changes {#network-connectivity-changes}

Adobe AIR 1.0 and later

Your AIR application can run in environments with uncertain and changing network connectivity. To help an application manage connections to online resources, Adobe AIR sends a network change event whenever a network connection becomes available or unavailable. Both the NetworkInfo object and the application’s NativeApplication object dispatch the networkChange event. To react to this event, add a listener:

NetworkInfo.networkInfo.addEventListener(Event.NETWORK_CHANGE, onNetworkChange);

And define an event handler function:

function onNetworkChange(event:Event)

{

//Check resource availability

}

The networkChange event does not indicate a change in all network activity, only that an individual network connection has changed. AIR does not attempt to interpret the meaning of the network change. A networked computer can have many real and virtual connections, so losing a connection does not necessarily mean losing a resource. On the other hand, new connections do not guarantee improved resource availability, either. Sometimes a new connection can even block access to resources previously available (for example, when connecting to a VPN).

In general, the only way for an application to determine whether it can connect to a remote resource is to try it. The service monitoring framework provides an event-based means of responding to changes in network connectivity to a specified host.

**_Note:_** _The service monitoring framework detects whether a server responds acceptably to a request. A successful check does not guarantee full connectivity. Scalable web services often use caching and load-balancing appliances to redirect traffic to a cluster of web servers. In this situation, service providers only provide a partial diagnosis of network connectivity._

**Service monitoring**

Adobe AIR 1.0 and later

The service monitor framework, separate from the AIR framework, resides in the file aircore.swc. To use the framework, the aircore.swc file must be included in your build process.

Adobe® Flash® Builder includes this library automatically.

The ServiceMonitor class implements the framework for monitoring network services and provides a base functionality for service monitors. By default, an instance of the ServiceMonitor class dispatches events regarding network connectivity. The ServiceMonitor object dispatches these events when the instance is created and whenever the runtime detects a network change. Additionally, you can set the pollInterval property of a ServiceMonitor instance to check connectivity at a specified interval in milliseconds, regardless of general network connectivity events. A ServiceMonitor object does not check network connectivity until the start() method is called.

The URLMonitor class, a subclass of the ServiceMonitor class, detects changes in HTTP connectivity for a specified URLRequest.

The SocketMonitor class, also a subclass of the ServiceMonitor class, detects changes in connectivity to a specified host at a specified port.

**_Note:_** _Prior to AIR 2, the service monitor framework was published in the servicemonitor.swc library. This library is now deprecated. Use the aircore.swc library instead._

Flash CS4 and CS5 Professional

To use these classes in Adobe® Flash® CS4 or CS5 Professional:

1.  Select the File &gt; Publish Settings command.
2.  Click the Settings button for Haxe\. Select Library Path.
3.  Click the Browse to SWC button and browse to the AIK folder in your Flash Professional installation folder.
4.  Within this folder, find the /frameworks/libs/air/aircore.swc (for AIR 2) or

/frameworks/libs/air/servicemonitor.swc (for AIR 1.5).

1.  Click the OK button.
2.  Add the following import statement to your Haxe code:

import air.net.*;

Flash CS3 Professional

To use these classes in Adobe® Flash® CS3 Professional, drag the ServiceMonitorShim component from the Components panel to the Library. Then, add the following importstatement to your Haxe code:

import air.net.*;

## HTTP monitoring {#http-monitoring}

Adobe AIR 1.0 and later

The URLMonitor class determines if HTTP requests can be made to a specified address at port 80 (the typical port for HTTP communication). The following code uses an instance of the URLMonitor class to detect connectivity changes to the Adobe website:

import air.net.URLMonitor;
import openfl.net.URLRequest;
import openfl.events.StatusEvent;

var monitor:URLMonitor;

monitor = new URLMonitor(new URLRequest('http://www.example.com')); monitor.addEventListener(StatusEvent.STATUS, announceStatus); monitor.start();

function announceStatus(e:StatusEvent):void {

trace("Status change. Current status: " + monitor.available);

}

## Socket monitoring {#socket-monitoring}

Adobe AIR 1.0 and later

AIR applications can also use socket connections for push-model connectivity. Firewalls and network routers typically restrict network communication on unauthorized ports for security reasons. For this reason, developers must consider that users do not always have the capability to make socket connections.

The following code uses an instance of the SocketMonitor class to detect connectivity changes to a socket connection. The port monitored is 6667, a common port for IRC:

import air.net.ServiceMonitor;
import openfl.events.StatusEvent;

socketMonitor = new SocketMonitor('www.example.com',6667);
socketMonitor.addEventListener(StatusEvent.STATUS, socketStatusChange);
socketMonitor.start();

function announceStatus(e:StatusEvent):void {

trace("Status change. Current status: " + socketMonitor.available);

}

If the socket server requires a secure connection, you can use the SecureSocketMonitor class instead of SocketMonitor.