# Chapter 41: Working with byte arrays {#chapter-41-working-with-byte-arrays}

The ByteArray class allows you to read from and write to a binary stream of data, which is essentially an array of bytes. This class provides a way to access data at the most elemental level. Because computer data consists of bytes, or groups of 8 bits, the ability to read data in bytes means that you can access data for which classes and access methods do not exist. The ByteArray class allows you to parse any stream of data, from a bitmap to a stream of data traveling over the network, at the byte level.

The writeObject() method allows you to write an object in serialized Action Message Format (AMF) to a ByteArray, while the readObject() method allows you to read a serialized object from a ByteArray to a variable of the original data type. You can serialize any object except for display objects, which are those objects that can be placed on the display list. You can also assign serialized objects back to custom class instances if the custom class is available to the runtime. After converting an object to AMF, you can efficiently transfer it over a network connection or save it to a file.

The sample Adobe® AIR® application described here reads a .zip file as an example of processing a byte stream, extracting a list of the files that the .zip file contains and writing them to the desktop.

**More Help topics**

[flash.utils.ByteArray](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/utils/ByteArray.html) [flash.utils.IExternalizable](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/utils/IExternalizable.html)

[Action Message Format specification](http://opensource.adobe.com/wiki/download/attachments/1114283/amf3_spec_05_05_08.pdf)

**Reading and writing a ByteArray**

The ByteArray class is part of the flash.utils package. To create a ByteArray object in Haxe, import the ByteArray class and invoke the constructor, as shown in the following example:

import flash.utils.ByteArray;

var stream:ByteArray = new ByteArray();

**ByteArray methods**

Any meaningful data stream is organized into a format that you can analyze to find the information that you want. A record in a simple employee file, for example, would probably include an ID number, a name, an address, a phone number, and so on. An MP3 audio file contains an ID3 tag that identifies the title, author, album, publishing date, and genre of the file that’s being downloaded. The format allows you to know the order in which to expect the data on the data stream. It allows you to read the byte stream intelligently.

The ByteArray class includes several methods that make it easier to read from and write to a data stream. Some of these methods include readBytes() and writeBytes(), readInt() and writeInt(), readFloat() and writeFloat(), readObject() and writeObject(), and readUTFBytes() and writeUTFBytes(). These methods enable you to read data from the data stream into variables of specific data types and write from specific data types directly to the binary data stream.

For example, the following code reads a simple array of strings and floating-point numbers and writes each element to a ByteArray. The organization of the array allows the code to call the appropriate ByteArray methods (writeUTFBytes() and writeFloat()) to write the data. The repeating data pattern makes it possible to read the array with a loop.

// The following example reads a simple Array (groceries), made up of strings

// and floating-point numbers, and writes it to a ByteArray. import flash.utils.ByteArray;

// define the grocery list Array

var groceries:Array = [&quot;milk&quot;, 4.50, &quot;soup&quot;, 1.79, &quot;eggs&quot;, 3.19, &quot;bread&quot; , 2.35]

// define the ByteArray

var bytes:ByteArray = new ByteArray();

// for each item in the array

for (var i:int = 0; i &lt; groceries.length; i++) {

bytes.writeUTFBytes(groceries[i++]); //write the string and position to the next item bytes.writeFloat(groceries[i]);// write the float

trace(&quot;bytes.position is: &quot; + bytes.position);//display the position in ByteArray

}

trace(&quot;bytes length is: &quot; + bytes.length);// display the length

## The position property {#the-position-property}

The position property stores the current position of the pointer that indexes the ByteArray during reading or writing. The initial value of the position property is 0 (zero) as shown in the following code:

var bytes:ByteArray = new ByteArray();

trace(&quot;bytes.position is initially: &quot; + bytes.position); // 0

When you read from or write to a ByteArray, the method that you use updates the position property to point to the location immediately following the last byte that was read or written. For example, the following code writes a string to a ByteArray and afterward the position property points to the byte immediately following the string in the ByteArray:

var bytes:ByteArray = new ByteArray();

trace(&quot;bytes.position is initially: &quot; + bytes.position); // 0 bytes.writeUTFBytes(&quot;Hello World!&quot;);

trace(&quot;bytes.position is now: &quot; + bytes.position);// 12

Likewise, a read operation increments the position property by the number of bytes read.

var bytes:ByteArray = new ByteArray();

trace(&quot;bytes.position is initially: &quot; + bytes.position); // 0 bytes.writeUTFBytes(&quot;Hello World!&quot;);

trace(&quot;bytes.position is now: &quot; + bytes.position);// 12 bytes.position = 0;

trace(&quot;The first 6 bytes are: &quot; + (bytes.readUTFBytes(6)));//Hello trace(&quot;And the next 6 bytes are: &quot; + (bytes.readUTFBytes(6)));// World!

Notice that you can set the position property to a specific location in the ByteArray to read or write at that offset.

## The bytesAvailable and length properties {#the-bytesavailable-and-length-properties}

The length and bytesAvailable properties tell you how long a ByteArray is and how many bytes remain in it from the current position to the end. The following example illustrates how you can use these properties. The example writes a String of text to the ByteArray and then reads the ByteArray one byte at a time until it encounters either the character “a” or the end (bytesAvailable &lt;= 0).

var bytes:ByteArray = new ByteArray();

var text:String = &quot;Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Vivamus etc.&quot;;

bytes.writeUTFBytes(text); // write the text to the ByteArray trace(&quot;The length of the ByteArray is: &quot; + bytes.length);// 70 bytes.position = 0; // reset position

while (bytes.bytesAvailable &gt; 0 &amp;&amp; (bytes.readUTFBytes(1) != &#039;a&#039;)) {

//read to letter a or end of bytes

}

if (bytes.position &lt; bytes.bytesAvailable) {

trace(&quot;Found the letter a; position is: &quot; + bytes.position); // 23 trace(&quot;and the number of bytes available is: &quot; + bytes.bytesAvailable);// 47

}

## The endian property {#the-endian-property}

Computers can differ in how they store multibyte numbers, that is, numbers that require more than 1 byte of memory to store them. An integer, for example, can take 4 bytes, or 32 bits, of memory. Some computers store the most significant byte of the number first, in the lowest memory address, and others store the least significant byte first. This attribute of a computer, or of byte ordering, is referred to as being either _big endian_ (most significant byte first) or _little endian_ (least significant byte first). For example, the number 0x31323334 would be stored as follows for big endian and little endian byte ordering, where a0 represents the lowest memory address of the 4 bytes and a3 represents the highest:

| **Big Endian** | **Big Endian** | **Big Endian** | **Big Endian** |
| --- | --- | --- | --- |
| a0 | a1 | a2 | a3 |
| 31 | 32 | 33 | 34 |

| **Little Endian** | **Little Endian** | **Little Endian** | **Little Endian** |
| --- | --- | --- | --- |
| a0 | a1 | a2 | a3 |
| 34 | 33 | 32 | 31 |

The endian property of the ByteArray class allows you to denote this byte order for multibyte numbers that you are processing. The acceptable values for this property are either &quot;bigEndian&quot; or &quot;littleEndian&quot; and the Endian class defines the constants BIG_ENDIAN and LITTLE_ENDIAN for setting the endian property with these strings.

## The compress() and uncompress() methods {#the-compress-and-uncompress-methods}

The compress() method allows you to compress a ByteArray in accordance with a compression algorithm that you specify as a parameter. The uncompress() method allows you to uncompress a compressed ByteArray in accordance with a compression algorithm. After calling compress() and uncompress(), the length of the byte array is set to the new length and the position property is set to the end.

The CompressionAlgorithm class defines constants that you can use to specify the compression algorithm. The ByteArray class supports the deflate (AIR-only), zlib, and lzma algorithms. The zlib compressed data format is described at [http://www.ietf.org/rfc/rfc1950.txt](http://www.ietf.org/rfc/rfc1950.txt). The lzma algorithm was added for OpenFL 11.4 and AIR 3.4\. It is described at [http://www.7-zip.org/7z.html](http://www.7-zip.org/7z.html).

The deflate compression algorithm is used in several compression formats, such as zlib, gzip, and some zip implementations. The deflate compression algorithm is described at [http://www.ietf.org/rfc/rfc1951.txt](http://www.ietf.org/rfc/rfc1951.txt).

The following example compresses a ByteArray called bytes using the lzma algorithm:

bytes.compress(CompressionAlgorithm.LZMA);

The following example uncompresses a compressed ByteArray using the deflate algorithm:

bytes.uncompress(CompressionAlgorithm.LZMA);

## Reading and writing objects {#reading-and-writing-objects}

The readObject() and writeObject() methods read an object from and write an object to a ByteArray, encoded in serialized Action Message Format (AMF). AMF is a proprietary message protocol created by Adobe and used by various Haxe classes, including Netstream, NetConnection, NetStream, LocalConnection, and Shared Objects.

A one-byte type marker describes the type of the encoded data that follows. AMF uses the following 13 data types:

value-type = undefined-marker | null-marker | false-marker | true-marker | integer-type | double-type | string-type | xml-doc-type | date-type | array-type | object-type |

xml-type | byte-array-type

The encoded data follows the type marker unless the marker represents a single possible value, such as null or true or false, in which case nothing else is encoded.

There are two versions of AMF: AMF0 and AMF3\. AMF 0 supports sending complex objects by reference and allows endpoints to restore object relationships. AMF 3 improves AMF 0 by sending object traits and strings by reference, in addition to object references, and by supporting new data types that were introduced in Haxe\. The ByteArray.objectEcoding property specifies the version of AMF that is used to encode the object data. The flash.net.ObjectEncoding class defines constants for specifying the AMF version: ObjectEncoding.AMF0 and ObjectEncoding.AMF3.

The following example calls writeObject() to write an XML object to a ByteArray, which it then compresses using the Deflate algorithm and writes to the order file on the desktop. The example uses a label to display the message “Wrote order file to desktop!” in the AIR window when it is finished.

import flash.filesystem.*; import flash.display.Sprite; import flash.display.TextField; import flash.utils.ByteArray;

public class WriteObjectExample extends Sprite

{

public function WriteObjectExample()

{

var bytes:ByteArray = new ByteArray(); var myLabel:TextField = new TextField(); myLabel.x = 150;

myLabel.y = 150;

myLabel.width = 200; addChild(myLabel);

var myXML:XML =

&lt;order&gt;

&lt;item id=&#039;1&#039;&gt;

&lt;menuName&gt;burger&lt;/menuName&gt;

&lt;price&gt;3.95&lt;/price&gt;

&lt;/item&gt;

&lt;item id=&#039;2&#039;&gt;

&lt;menuName&gt;fries&lt;/menuName&gt;

&lt;price&gt;1.45&lt;/price&gt;

&lt;/item&gt;

&lt;/order&gt;;

// Write XML object to ByteArray

bytes.writeObject(myXML);

bytes.position = 0;//reset position to beginning bytes.compress(CompressionAlgorithm.DEFLATE);// compress ByteArray writeBytesToFile(&quot;order.xml&quot;, bytes);

myLabel.text = &quot;Wrote order file to desktop!&quot;;

}

private function writeBytesToFile(fileName:String, data:ByteArray):void

{

var outFile:File = File.desktopDirectory; // dest folder is desktop outFile = outFile.resolvePath(fileName); // name of file to write var outStream:FileStream = new FileStream();

// open output file stream in WRITE mode outStream.open(outFile, FileMode.WRITE);

// write out the file outStream.writeBytes(data, 0, data.length);

// close it outStream.close();

}

}

The readObject() method reads an object in serialized AMF from a ByteArray and stores it in an object of the specified type. The following example reads the order file from the desktop into a ByteArray (inBytes), uncompresses it, and calls readObject() to store it in the XML object orderXML. The example uses a for each() loop construct to add each node to a text area for display. The example also displays the value of the objectEncoding property along with a header for the contents of the order file.

import flash.filesystem.*; import flash.display.Sprite; import flash.display.TextField; import flash.utils.ByteArray;

public class ReadObjectExample extends Sprite

{

public function ReadObjectExample()

{

var inBytes:ByteArray = new ByteArray();

// define text area for displaying XML content var myTxt:TextField = new TextField(); myTxt.width = 550;

myTxt.height = 400; addChild(myTxt);

//display objectEncoding and file heading

myTxt.text = &quot;Object encoding is: &quot; + inBytes.objectEncoding + &quot;\n\n&quot; + &quot;order file: \n\n&quot;; readFileIntoByteArray(&quot;order&quot;, inBytes);

inBytes.position = 0; // reset position to beginning inBytes.uncompress(CompressionAlgorithm.DEFLATE); inBytes.position = 0;//reset position to beginning

// read XML Object

var orderXML:XML = inBytes.readObject();

// for each node in orderXML

for each (var child:XML in orderXML)

{

// append child node to text area myTxt.text += child + &quot;\n&quot;;

}

}

// read specified file into byte array

private function readFileIntoByteArray(fileName:String, data:ByteArray):void

{

var inFile:File = File.desktopDirectory; // source folder is desktop inFile = inFile.resolvePath(fileName); // name of file to read

var inStream:FileStream = new FileStream(); inStream.open(inFile, FileMode.READ); inStream.readBytes(data);

inStream.close();

}

}