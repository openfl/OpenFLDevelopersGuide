# UDP sockets (AIR) {#udp-sockets-air}

Adobe AIR 2 and later

The Universal Datagram Protocol (UDP) provides a way to exchange messages over a stateless network connection. UDP provides no guarantees that messages are delivered in order or even that messages are delivered at all. With UDP, the operating system’s network code usually spends less time marshaling, tracking, and acknowledging messages.

Thus, UDP messages typically arrive at the destination application with a shorter delay than do TCP messages.

UDP socket communication is helpful when you must send real-time information such as position updates in a game, or sound packets in an audio chat application. In such applications, some data loss is acceptable, and low transmission latency is more important than guaranteed arrival. For almost all other purposes, TCP sockets are a better choice.

Your AIR application can send and receive UDP messages with the DatagramSocket and DatagramSocketDataEvent classes. To send or receive a UDP message:

1.  Create a DatagramSocket object
2.  Add an event listener for the data event
3.  Bind the socket to a local IP address and port using the bind() method
4.  Send messages by calling the send() method, passing in the IP address and port of the target computer
5.  Receive messages by responding to the data event. The DatagramSocketDataEvent object dispatched for this event contains a ByteArray object containing the message data.

The following code example illustrates how an application can send and receive UDP messages. The example sends a single message containing the string, “Hello.”, to the target computer. It also traces the contents of any messages received.

package

{

import flash.display.Sprite;

import flash.events.DatagramSocketDataEvent; import flash.events.Event;

import flash.net.DatagramSocket; import flash.utils.ByteArray;

public class DatagramSocketExample extends Sprite

{

private var datagramSocket:DatagramSocket;

//The IP and port for this computer

private var localIP:String = &quot;192.168.0.1&quot;; private var localPort:int = 55555;

//The IP and port for the target computer private var targetIP:String = &quot;192.168.0.2&quot;; private var targetPort:int = 55555;

public function DatagramSocketExample()

{

//Create the socket

datagramSocket = new DatagramSocket();

datagramSocket.addEventListener( DatagramSocketDataEvent.DATA, dataReceived );

//Bind the socket to the local network interface and port datagramSocket.bind( localPort, localIP );

//Listen for incoming datagrams datagramSocket.receive();

//Create a message in a ByteArray

var data:ByteArray = new ByteArray(); data.writeUTFBytes(&quot;Hello.&quot;);

//Send the datagram message

datagramSocket.send( data, 0, 0, targetIP, targetPort);

}

private function dataReceived( event:DatagramSocketDataEvent ):void

{

//Read the data from the datagram

trace(&quot;Received from &quot; + event.srcAddress + &quot;:&quot; + event.srcPort + &quot;&gt; &quot; + event.data.readUTFBytes( event.data.bytesAvailable ) );

}

}}

Keep in mind the following considerations when using UDP sockets:

*   A single packet of data cannot be larger than the smallest maximum transmission unit (MTU) of the network interface or any network nodes between the sender and the recipient. All of the data in the ByteArray object passed to the send() method is sent as a single packet. (In TCP, large messages are broken up into separate packets.)
*   There is no handshaking between the sender and the target. Messages are discarded without error if the target does not exist or does not have an active listener at the specified port.
*   When you use the connect() method, messages sent from other sources are ignored. A UDP connection provides convenient packet filtering only. It does not mean that there is necessarily a valid, listening process at the target address and port.
*   UDP traffic can swamp a network. Network administrators might need to implement quality-of-service controls if network congestion occurs. (TCP has built-in traffic control to reduce the impact of network congestion.)

For more information, see:

*   DatagramSocket
*   DatagramSocketDataEvent
*   ByteArray