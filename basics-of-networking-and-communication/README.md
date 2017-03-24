# Chapter 42: Basics of networking and communication {#chapter-42-basics-of-networking-and-communication}

When you build applications in OpenFL, you often need to access resources outside your application. For example, you might send a request for an image to an Internet web server and get the image data in return. Or, you might send serialized objects back and forth over a socket connection with an application server. The OpenFL APIs provide several classes that allow your applications to participate in this exchange. These APIs support IP- based networking for protocols like UDP, TCP, HTTP, RTMP, and RTMFP.

The following classes can be used to send and receive data across a network:

| **Class** | **Supported data formats** | **Protocols** | **Description** |
| --- | --- | --- | --- |
| [Loader](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/display/Loader.html) | SWF, PNG, JPEG, GIF | HTTP, HTTPS | Loads supported data types and converts the data into a display object. |
| URLLoader | Any (text, XML, binary, etc.) | HTTP, HTTPS | Loads arbitrary formats of data. Your application is responsible for interpreting the data. |
| [FileReference](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/net/FileReference.html) | Any | HTTP | Upload and download files. |
| NetConnection | Video, audio, Haxe Message Format (AMF) | HTTP, HTTPS, RTMP, RTMFP | Connects to video, audio and remote object streams. |
| Sound | Audio | HTTP | Loads and plays supported audio formats. |
| XMLSocket | XML | TCP | Exchanges XML messages with an XMLSocket server. |
| Socket | Any | TCP | Connects to a TCP socket server. |
| SecureSocket (AIR) | Any | TCP with SSLv3 or TLSv1 | Connects to a TCP socket server that requires SSL or TLS security. |
| ServerSocket (AIR) | Any | TCP | Acts as a server for incoming TCP socket connections. |
| DatagramSocket (AIR) | Any | UDP | Sends and receives UDP packets. |

Often, when creating a web application it is helpful to store persistent information about the user’s application state. HTML pages and applications typically use cookies for this purpose. In OpenFL, you can use the SharedObject class for the same purpose. See

“Shared objects” on page 701

. (The SharedObject class can be used in AIR applications, but there are fewer restrictions when just saving the data to a regular file.)

When your OpenFL application needs to communicate with another OpenFL application on the same computer, you can use the LocalConnection class. For example, two (or more) SWFs on the same web page can communicate with each other. Likewise, a SWF running on a web page can communicate with an AIR application. See

“Communicating with other OpenFL instances” on page 830

.

When you need to communicate with other, non-SWF processes on the local computer, you can use the NativeProcess class added in AIR 2\. The NativeProcess class allows your AIR application to launch and communicate with other applications. See

“Communicating with native processes in AIR” on page 837

.

When you need information about the network environment of the computer on which an AIR application is running, you can use the following classes:

*   NetworkInfo—Provides information about the available network interfaces, such as the computer’s IP address. See

    “Network interfaces” on page 791

    .
*   DNSResolver—Allows you to look up DNS records. See

    “Domain Name System (DNS) records” on page 794

    .
*   ServiceMonitor—Allows you to monitor the availability of a server. See

    “Service monitoring” on page 792

    .
*   URLMonitor—Allows you to monitor the availability of a resource at a particular URL. See

    “HTTP monitoring”

    on page 793

    .
*   SocketMonitor and SecureSocketMonitor—Allows you to monitor the availability of a resource at a socket. See

    “Socket monitoring” on page 794

    .

Important concepts and terms

The following reference list contains important terms that you will encounter when programming networking and communications code:

**External data** Data that is stored in some form outside of the application, and loaded into the application when needed. This data could be stored in a file that’s loaded directly, or stored in a database or other form that is retrieved by calling scripts or programs running on a server.

**URL-encoded variables** The URL-encoded format provides a way to represent several variables (pairs of variable names and values) in a single string of text. Individual variables are written in the format name=value. Each variable (that is, each name-value pair) is separated by ampersand characters, like this: variable1=value1&amp;variable2=value2\. In this way, an indefinite number of variables can be sent as a single message.

**MIME type** A standard code used to identify the type of a given file in Internet communication. Any given file type has a specific code that is used to identify it. When sending a file or message, a computer (such as a web server or a user’s OpenFL instance) will specify the type of file being sent.

**HTTP** Hypertext Transfer Protocol—a standard format for delivering web pages and various other types of content that are sent over the Internet.

**Request method** When an application (such as an AIR application or a web browser) sends a message (called an HTTP request) to a web server, any data being sent can be embedded in the request in one of two ways; these are the two request methods GET and POST. On the server end, the program receiving the request will need to look in the appropriate portion of the request to find the data, so the request method used to send data from your application should match the request method used to read that data on the server.

**Socket connection** A persistent connection for communication between two computers.

**Upload** To send a file to another computer.

**Download** To retrieve a file from another computer.

**Network interfaces**

Adobe AIR 2 and later

You can use the NetworkInfo object to discover the hardware and software network interfaces available to your application. The NetworkInfo object is a _singleton_ object, you do not need to create one. Instead, use the static class property, networkInfo, to access the single NetworkInfo object. The NetworkInfo object also dispatches a networkChange event when one of the available interfaces change.

Call the findInterfaces() method to get a list of NetworkInterface objects. Each NetworkInterface object in the list describes one of the available interfaces. The NetworkInterface object provides such information as the IP address, hardware address, maximum transmission unit, and whether the interface is active.

The following code example traces the NetworkInterface properties of each interface on the client computer:

package {

import flash.display.Sprite; import flash.net.InterfaceAddress; import flash.net.NetworkInfo; import flash.net.NetworkInterface;

public class NetworkInformationExample extends Sprite

{

public function NetworkInformationExample()

{

var networkInfo:NetworkInfo = NetworkInfo.networkInfo;

var interfaces:Vector.&lt;NetworkInterface&gt; = networkInfo.findInterfaces();

if( interfaces != null )

{

trace( &quot;Interface count: &quot; + interfaces.length );

for each ( var interfaceObj:NetworkInterface in interfaces )

{

trace( &quot;\nname: &quot; + interfaceObj.name );

trace( &quot;display name: &quot; + interfaceObj.displayName ); trace( &quot;mtu: &quot; + interfaceObj.mtu );

trace( &quot;active?: &quot; + interfaceObj.active );

trace( &quot;parent interface: &quot; + interfaceObj.parent );

trace( &quot;hardware address: &quot; + interfaceObj.hardwareAddress ); if( interfaceObj.subInterfaces != null )

{

trace( &quot;# subinterfaces: &quot; + interfaceObj.subInterfaces.length );

}

trace(&quot;# addresses: &quot; + interfaceObj.addresses.length );

for each ( var address:InterfaceAddress in interfaceObj.addresses )

{

trace( &quot; type: &quot; + address.ipVersion ); trace( &quot; address: &quot; + address.address );

trace( &quot; broadcast: &quot; + address.broadcast );

trace( &quot; prefix length: &quot; + address.prefixLength );

}

}

}

}

}

}

For more information, see:

*   NetworkInfo
*   NetworkInterface
*   InterfaceAddress
*   [Flexpert: Detecting the network connection type with Flex 4.5](http://www.flexpert.be/2011/04/detecting-the-network-connection-type-with-flex-4-5/)