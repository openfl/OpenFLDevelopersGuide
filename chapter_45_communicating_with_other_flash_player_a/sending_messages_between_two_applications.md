## Sending messages between two applications {#sending-messages-between-two-applications}

Flash Player 9 and later, Adobe AIR 1.0 and later

You use the LocalConnection class to communicate between different AIR applications and between different Adobe® Flash® Player (SWF) applications running in a browser. You can also use the LocalConnection class to communicate between an AIR application and a SWF application running in a browser.

For example, you could have multiple Flash Player instances on a web page, or have a Flash Player instance retrieve data from a Flash Player instance in a pop-up window.

The following code defines a LocalConnection object that acts as a server and accepts incoming LocalConnection calls from other applications:

package

{

import flash.net.LocalConnection; import flash.display.Sprite;

public class ServerLC extends Sprite

{

public function ServerLC()

{

var lc:LocalConnection = new LocalConnection(); lc.client = new CustomClient1();

try

{

lc.connect(&quot;conn1&quot;);

}

catch (error:Error)

{

trace(&quot;error:: already connected&quot;);

}

}

}

}

This code first creates a LocalConnection object named lc and sets the client property to an object, clientObject. When another application calls a method in this LocalConnection instance, the runtime looks for that method in the clientObject object.

If you already have a connection with the specified name, an Argument Error exception is thrown, indicating that the connection attempt failed because the object is already connected.

Whenever a Flash Player instance connects to this SWF file and tries to invoke any method for the specified local connection, the request is sent to the class specified by the client property, which is set to the CustomClient1 class:

package

{

import flash.events.*;

import flash.system.fscommand; import flash.utils.Timer;

public class CustomClient1 extends Object

{

public function doMessage(value:String = &quot;&quot;):void

{

trace(value);

}

public function doQuit():void

{

trace(&quot;quitting in 5 seconds&quot;); this.close();

var quitTimer:Timer = new Timer(5000, 1); quitTimer.addEventListener(TimerEvent.TIMER, closeHandler);

}

public function closeHandler(event:TimerEvent):void

{

fscommand(&quot;quit&quot;);

}

}

}

To create a LocalConnection server, call the LocalConnection.connect() method and provide a unique connection name. If you already have a connection with the specified name, an ArgumentError error is generated, indicating that the connection attempt failed because the object is already connected.

The following snippet demonstrates how to create a LocalConnection with the name conn1: try

{

connection.connect(&quot;conn1&quot;);

}

catch (error:ArgumentError)

{

trace(&quot;Error! Server already exists\n&quot;);

}

Connecting to the primary application from a secondary application requires that you first create a LocalConnection object in the sending LocalConnection object; then call the LocalConnection.send() method with the name of the connection and the name of the method to execute. For example, to send the doQuit method to the LocalConnection object that you created earlier, use the following code:

sendingConnection.send(&quot;conn1&quot;, &quot;doQuit&quot;);

This code connects to an existing LocalConnection object with the connection name conn1 and invokes the doMessage() method in the remote application. If you want to send parameters to the remote application, you specify additional arguments after the method name in the send() method, as the following snippet shows:

sendingConnection.send(&quot;conn1&quot;, &quot;doMessage&quot;, &quot;Hello world&quot;);