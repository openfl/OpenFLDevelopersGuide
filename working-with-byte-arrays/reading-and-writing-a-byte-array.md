# Reading and writing a ByteArray {#reading-and-writing-a-byte-array}

The [ByteArray class](https://api.openfl.org/openfl/utils/ByteArray.html) is
part of the
[openfl.utils package](https://api.openfl.org/openfl/utils/index.html). To
create a ByteArray object in Haxe, import the ByteArray class and invoke the
constructor, as shown in the following example:

```haxe
import openfl.display.Sprite;
import openfl.utils.ByteArray;

class CreateByteArrayExample extends Sprite {
	public function new() {
		super();

	    var stream:ByteArray = new ByteArray();
	}
}
```

## ByteArray methods

Any meaningful data stream is organized into a format that you can analyze to
find the information that you want. A record in a simple employee file, for
example, would probably include an ID number, a name, an address, a phone
number, and so on. An MP3 audio file contains an ID3 tag that identifies the
title, author, album, publishing date, and genre of the file that's being
downloaded. The format allows you to know the order in which to expect the data
on the data stream. It allows you to read the byte stream intelligently.

The ByteArray class includes several methods that make it easier to read from
and write to a data stream. Some of these methods include `readBytes()` and
`writeBytes()`, `readInt()` and `writeInt()`, `readFloat()` and `writeFloat()`,
`readObject()` and `writeObject()`, and `readUTFBytes()` and `writeUTFBytes()`.
These methods enable you to read data from the data stream into variables of
specific data types and write from specific data types directly to the binary
data stream.

For example, the following code reads a simple array of strings and
floating-point numbers and writes each element to a ByteArray. The organization
of the array allows the code to call the appropriate ByteArray methods
(`writeUTFBytes()` and `writeFloat()`) to write the data. The repeating data
pattern makes it possible to read the array with a loop.

```haxe
// The following example reads a simple Array (groceries), made up of strings
// and floating-point numbers, and writes it to a ByteArray.
import openfl.display.Sprite;
import openfl.utils.ByteArray;

class WriteByteArrayExample extends Sprite {
	public function new() {
		super();

		// define the grocery list Array
		var groceries:Array<Dynamic> = ["milk", 4.50, "soup", 1.79, "eggs", 3.19, "bread", 2.35];
		// define the ByteArray
		var bytes:ByteArray = new ByteArray();
		// for each item in the array
		var i = 0;
		while (i < groceries.length) {
			bytes.writeUTFBytes(groceries[i++]); // write the string and position to the next item
			bytes.writeFloat(groceries[i++]); // write the float
			trace("bytes.position is: " + bytes.position); // display the position in ByteArray
		}
		trace("bytes length is: " + bytes.length); // display the length
	}
}

```

## The position property

The position property stores the current position of the pointer that indexes
the ByteArray during reading or writing. The initial value of the position
property is 0 (zero) as shown in the following code:

```haxe
var bytes:ByteArray = new ByteArray();
trace("bytes.position is initially: " + bytes.position);     // 0
```

When you read from or write to a ByteArray, the method that you use updates the
position property to point to the location immediately following the last byte
that was read or written. For example, the following code writes a string to a
ByteArray and afterward the position property points to the byte immediately
following the string in the ByteArray:

```haxe
var bytes:ByteArray = new ByteArray();
trace("bytes.position is initially: " + bytes.position);     // 0
bytes.writeUTFBytes("Hello World!");
trace("bytes.position is now: " + bytes.position);    // 12
```

Likewise, a read operation increments the position property by the number of
bytes read.

```haxe
var bytes:ByteArray = new ByteArray();

trace("bytes.position is initially: " + bytes.position);     // 0
bytes.writeUTFBytes("Hello World!");
trace("bytes.position is now: " + bytes.position);    // 12
bytes.position = 0;
trace("The first 6 bytes are: " + (bytes.readUTFBytes(6)));    //Hello
trace("And the next 6 bytes are: " + (bytes.readUTFBytes(6)));    // World!
```

Notice that you can set the position property to a specific location in the
ByteArray to read or write at that offset.

## The bytesAvailable and length properties

The `length` and `bytesAvailable` properties tell you how long a ByteArray is
and how many bytes remain in it from the current position to the end. The
following example illustrates how you can use these properties. The example
writes a String of text to the ByteArray and then reads the ByteArray one byte
at a time until it encounters either the character "a" or the end
(`bytesAvailable <= 0`).

```haxe
import openfl.display.Sprite;
import openfl.utils.ByteArray;

class BytesAvailableAndLengthExample extends Sprite {
	public function new() {
		super();

		var bytes:ByteArray = new ByteArray();
		var text:String = "Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Vivamus etc.";

		bytes.writeUTFBytes(text); // write the text to the ByteArray
		trace("The length of the ByteArray is: " + bytes.length); // 70
		bytes.position = 0; // reset position
		while (bytes.bytesAvailable > 0 && (bytes.readUTFBytes(1) != 'a')) {
			// read to letter a or end of bytes
		}
		if (bytes.position < bytes.bytesAvailable) {
			trace("Found the letter a; position is: " + bytes.position); // 23
			trace("and the number of bytes available is: " + bytes.bytesAvailable); // 47
		}
	}
}

```

## The endian property

Computers can differ in how they store multibyte numbers, that is, numbers that
require more than 1 byte of memory to store them. An integer, for example, can
take 4 bytes, or 32 bits, of memory. Some computers store the most significant
byte of the number first, in the lowest memory address, and others store the
least significant byte first. This attribute of a computer, or of byte ordering,
is referred to as being either _big endian_ (most significant byte first) or
_little endian_ (least significant byte first). For example, the number
0x31323334 would be stored as follows for big endian and little endian byte
ordering, where a0 represents the lowest memory address of the 4 bytes and a3
represents the highest:

| Big Endian | Big Endian | Big Endian | Big Endian |
| ---------- | ---------- | ---------- | ---------- |
| a0         | a1         | a2         | a3         |
| 31         | 32         | 33         | 34         |

| Little Endian | Little Endian | Little Endian | Little Endian |
| ------------- | ------------- | ------------- | ------------- |
| a0            | a1            | a2            | a3            |
| 34            | 33            | 32            | 31            |

The `endian` property of the ByteArray class allows you to denote this byte
order for multibyte numbers that you are processing. The acceptable values for
this property are either `"bigEndian"` or `"littleEndian"` and the Endian class
defines the constants `BIG_ENDIAN` and `LITTLE_ENDIAN` for setting the `endian`
property with these strings.

## The compress() and uncompress() methods

The `compress()` method allows you to compress a ByteArray in accordance with a
compression algorithm that you specify as a parameter. The `uncompress()` method
allows you to uncompress a compressed ByteArray in accordance with a compression
algorithm. After calling `compress()` and `uncompress()`, the length of the byte
array is set to the new length and the position property is set to the end.

The CompressionAlgorithm class defines constants that you can use to specify the
compression algorithm. The ByteArray class supports the deflate (AIR-only),
zlib, and lzma algorithms. The zlib compressed data format is described at
<http://www.ietf.org/rfc/rfc1950.txt>. The lzma algorithm is described at
<http://www.7-zip.org/7z.html>.

The deflate compression algorithm is used in several compression formats, such
as zlib, gzip, and some zip implementations. The deflate compression algorithm
is described at <http://www.ietf.org/rfc/rfc1951.txt>.

The following example compresses a ByteArray called `bytes` using the lzma
algorithm:

```haxe
bytes.compress(CompressionAlgorithm.LZMA);
```

The following example uncompresses a compressed ByteArray using the deflate
algorithm:

```haxe
bytes.uncompress(CompressionAlgorithm.LZMA);
```

## Reading and writing objects

The `readObject()` and `writeObject()` methods read an object from and write an
object to a ByteArray, encoded in a serialized format, such as
[Haxe Serialization Format](https://haxe.org/manual/std-serialization.html),
JSON, or Action Message Format (AMF). These object serialization formats are
used by various OpenFL classes, including NetConnection, NetStream,
LocalConnection, and Shared Objects.

The `ByteArray.objectEncoding` property specifies the serialization format that
is used to encode the object data. The openfl.net.ObjectEncoding class defines
constants for specifying the serialization format:

- `ObjectEncoding.HXSF`
- `ObjectEncoding.JSON`
- `ObjectEncoding.AMF0`
- `ObjectEncoding.AMF3`

The following example calls `writeObject()` to write an XML object to a
ByteArray, which it then compresses using the Deflate algorithm and writes to
the `order` file on the desktop. The example uses a label to display the message
"Wrote order file to desktop!" in the OpenFL window when it is finished.

```haxe
import openfl.filesystem.*;
import openfl.display.Sprite;
import openfl.text.TextField;
import openfl.utils.ByteArray;
import openfl.utils.CompressionAlgorithm;

class WriteObjectExample extends Sprite {
	public function new() {
		super();

		var bytes:ByteArray = new ByteArray();
		var myLabel:TextField = new TextField();
		myLabel.x = 150;
		myLabel.y = 150;
		myLabel.width = 200;
		addChild(myLabel);

		var order:Dynamic = {
			items: [
				{id: '1', menuName: 'burger', price: 3.95},
				{id: '2', menuName: 'fries', price: 1.45},
			]
		};

		// Write order object to ByteArray
		bytes.writeObject(order);
		bytes.position = 0; // reset position to beginning
		bytes.compress(CompressionAlgorithm.DEFLATE); // compress ByteArray
		writeBytesToFile("order.dat", bytes);
		myLabel.text = "Wrote order file to desktop!";
	}

	function writeBytesToFile(fileName:String, data:ByteArray):Void {
		var outFile:File = File.desktopDirectory; // dest folder is desktop
		outFile = outFile.resolvePath(fileName); // name of file to write
		var outStream:FileStream = new FileStream();
		// open output file stream in WRITE mode
		outStream.open(outFile, FileMode.WRITE);
		// write out the file
		outStream.writeBytes(data, 0, data.length);
		// close it
		outStream.close();
	}
}
```

The `readObject()` method reads an object in serialized format from a ByteArray
and stores it in an object of the specified type. The following example reads
the `order` file from the desktop into a ByteArray (`inBytes`), uncompresses it,
and calls `readObject()` to store it in the XML object `orderXML`. The example
uses a loop to add each node to a text area for display. The example also
displays the value of the `objectEncoding` property along with a header for the
contents of the `order` file.

```haxe
import openfl.filesystem.*;
import openfl.display.Sprite;
import openfl.text.TextField;
import openfl.utils.ByteArray;
import openfl.utils.CompressionAlgorithm;

class ReadObjectExample extends Sprite {
	public function new() {
		super();

		var inBytes:ByteArray = new ByteArray();
		// define text area for displaying XML content
		var myTxt:TextField = new TextField();
		myTxt.width = 550;
		myTxt.height = 400;
		addChild(myTxt);
		// display objectEncoding and file heading
		myTxt.text = "Object encoding is: " + inBytes.objectEncoding + "\n\n" + "order file: \n\n";
		readFileIntoByteArray("order.dat", inBytes);

		inBytes.position = 0; // reset position to beginning
		inBytes.uncompress(CompressionAlgorithm.DEFLATE);
		inBytes.position = 0; // reset position to beginning
		// read order object
		var order:Dynamic = inBytes.readObject();

		// for each node in orderXML
		var items:Array<Dynamic> = order.items;
		for (item in items) {
			// append item node to text area
			myTxt.text += item.id + " " + item.menuName + " " + item.price + "\n";
		}
	}

	// read specified file into byte array
	function readFileIntoByteArray(fileName:String, data:ByteArray):Void {
		var inFile:File = File.desktopDirectory; // source folder is desktop
		inFile = inFile.resolvePath(fileName); // name of file to read
		var inStream:FileStream = new FileStream();
		inStream.open(inFile, FileMode.READ);
		inStream.readBytes(data);
		inStream.close();
	}
}
```
