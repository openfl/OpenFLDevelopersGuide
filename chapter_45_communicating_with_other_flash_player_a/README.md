# Chapter 45: Communicating with other Flash Player and AIR instances {#chapter-45-communicating-with-other-flash-player-and-air-instances}

Flash Player 9 and later, Adobe AIR 1.0 and later

The LocalConnection class enables communications between Adobe® AIR® applications, as well as between SWF content running in the browser. You can also use the LocalConnection class to communicate between an AIR application and SWF content running in the browser. The LocalConnection class allows you to build versatile applications that can share data between Flash Player and AIR instances.

**About the LocalConnection class**

Flash Player 9 and later, Adobe AIR 1.0 and later

The LocalConnection class lets you develop SWF files that can send instructions to other SWF files without the use of the fscommand() method or JavaScript. LocalConnection objects can communicate only among SWF files that are running on the same client computer, but they can run in different applications. For example, a SWF file running in a browser and a SWF file running in a projector can share information, with the projector maintaining local information and the browser-based SWF file connecting remotely. (A projector is a SWF file saved in a format that can run as a stand-alone application—that is, the projector doesn’t require Flash Player to be installed because it is embedded inside the executable.)

LocalConnection objects can be used to communicate between SWFs using different ActionScript versions:

*   ActionScript 3.0 LocalConnection objects can communicate with LocalConnection objects created in ActionScript 1.0 or 2.0.
*   ActionScript 1.0 or 2.0 LocalConnection objects can communicate with LocalConnection objects created in ActionScript 3.0.

Flash Player handles this communication between LocalConnection objects of different versions automatically.

The simplest way to use a LocalConnection object is to allow communication only between LocalConnection objects located in the same domain or the same AIR application. That way, you do not have to worry about security issues. However, if you need to allow communication between domains, you have several ways to implement security measures. For more information, see the discussion of the connectionName parameter of the send() method and the allowDomain() and domain entries in the LocalConnection class listing in the [ActionScript 3.0 Reference for the](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/net/LocalConnection.html) [Adobe Flash Platform](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/net/LocalConnection.html).

_It is possible to use LocalConnection objects to send and receive data within a single SWF file, but Adobe does not recommended doing so. Instead, use shared objects._

There are three ways to add callback methods to your LocalConnection objects:

*   Subclass the LocalConnection class and add methods.
*   Set the LocalConnection.client property to an object that implements the methods.
*   Create a dynamic class that extends LocalConnection and dynamically attach methods.

The first way to add callback methods is to extend the LocalConnection class. You define the methods within the custom class instead of dynamically adding them to the LocalConnection instance. This approach is demonstrated in the following code:

package

{

import flash.net.LocalConnection;

public class CustomLocalConnection extends LocalConnection

{

public function CustomLocalConnection(connectionName:String)

{

try

{

}

connect(connectionName);

catch (error:ArgumentError)

{

// server already created/connected

}

}

public function onMethod(timeString:String):void

{

trace(&quot;onMethod called at: &quot; + timeString);

}

}

}

In order to create a new instance of the CustomLocalConnection class, you can use the following code:

var serverLC:CustomLocalConnection;

serverLC = new CustomLocalConnection(&quot;serverName&quot;);

The second way to add callback methods is to use the LocalConnection.client property. This involves creating a custom class and assigning a new instance to the client property, as the following code shows:

var lc:LocalConnection = new LocalConnection(); lc.client = new CustomClient();

The LocalConnection.client property indicates the object callback methods that should be invoked. In the previous code, the client property was set to a new instance of a custom class, CustomClient. The default value for the client property is the current LocalConnection instance. You can use the client property if you have two data handlers that have the same set of methods but act differently—for example, in an application where a button in one window toggles the view in a second window.

To create the CustomClient class, you could use the following code:

package

{

public class CustomClient extends Object

{

public function onMethod(timeString:String):void

{

trace(&quot;onMethod called at: &quot; + timeString);

}

}

}

The third way to add callback methods, creating a dynamic class and dynamically attaching the methods, is very similar to using the LocalConnection class in earlier versions of ActionScript, as the following code shows:

import flash.net.LocalConnection;

dynamic class DynamicLocalConnection extends LocalConnection {}

Callback methods can be dynamically added to this class by using the following code:

var connection:DynamicLocalConnection = new DynamicLocalConnection(); connection.onMethod = this.onMethod;

// Add your code here.

public function onMethod(timeString:String):void

{

trace(&quot;onMethod called at: &quot; + timeString);

}

The previous way of adding callback methods is not recommended because the code is not very portable. In addition, using this method of creating local connections could create performance issues, because accessing dynamic properties is dramatically slower than accessing sealed properties.

isPerUser property

The isPerUser property was added to Flash Player (10.0.32) and AIR (1.5.2) to resolve a conflict that occurs when more than one user is logged into a Mac computer. On other operating systems, the property is ignored since the local connection has always been scoped to individual users. The isPerUser property should be set to true in new code. However, the default value is currently false for backward compatibility. The default may be changed in future versions of the runtimes.