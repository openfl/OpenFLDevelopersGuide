# The read buffer and the bytesAvailable property of a FileStream object

When a
[FileStream object](https://api.openfl.org/openfl/filesystem/FileStream.html)
with read capabilities (one in which the `fileMode` parameter of the `open()` or
`openAsync()` method was set to `READ` or `UPDATE`) is opened, the runtime
stores the data in an internal buffer. The FileStream object begins reading data
into the buffer as soon as you open the file (by calling the `open()` or
`openAsync()` method of the FileStream object).

For a file opened for synchronous operations (using the `open()` method), you
can always set the `position` pointer to any valid position (within the bounds
of the file) and begin reading any amount of data (within the bounds of the
file), as shown in the following code (which assumes that the file contains at
least 100 bytes):

```haxe
import openfl.display.Sprite;
import openfl.filesystem.File;
import openfl.filesystem.FileMode;
import openfl.filesystem.FileStream;
import openfl.utils.ByteArray;

class FileStreamReadBufferExample1 extends Sprite
{
    public function new()
    {
        super();

        var myByteArray = new ByteArray();

        var myFile:File = File.documentsDirectory.resolvePath("OpenFL Test/test.txt");
        var myFileStream:FileStream = new FileStream();
        myFileStream.open(myFile, FileMode.READ);
        myFileStream.position = 10;
        myFileStream.readBytes(myByteArray, 0, 20);
        myFileStream.position = 89;
        myFileStream.readBytes(myByteArray, 0, 10);
    }
}
```

Whether a file is opened for synchronous or asynchronous operations, the read
methods always read from the "available" bytes, represented by the
`bytesAvalable` property. When reading synchronously, all of the bytes of the
file are available all of the time. When reading asynchronously, the bytes
become available starting at the position specified by the `position` property,
in a series of asynchronous buffer fills signaled by `progress` events.

For files opened for _synchronous_ operations, the `bytesAvailable` property is
always set to represent the number of bytes from the `position` property to the
end of the file (all bytes in the file are always available for reading).

For files opened for _asynchronous_ operations, you need to ensure that the read
buffer has consumed enough data before calling a read method. For a file opened
asynchronously, as the read operation progresses, the data from the file,
starting at the `position` specified when the read operation started, is added
to the buffer, and the `bytesAvailable` property increments with each byte read.
The `bytesAvailable` property indicates the number of bytes available starting
with the byte at the position specified by the `position` property to the end of
the buffer. Periodically, the FileStream object sends a `progress` event.

For a file opened asynchronously, as data becomes available in the read buffer,
the FileStream object periodically dispatches the `progress` event. For example,
the following code reads data into a ByteArray object, `bytes`, as it is read
into the buffer:

```haxe
import openfl.display.Sprite;
import openfl.events.ProgressEvent;
import openfl.filesystem.File;
import openfl.filesystem.FileMode;
import openfl.filesystem.FileStream;
import openfl.utils.ByteArray;

class FileStreamReadBufferExample2 extends Sprite
{
    private var bytes:ByteArray;
    private var myFileStream:FileStream;

    public function new()
    {
        super();

        bytes = new ByteArray();
        var myFile:File = File.documentsDirectory.resolvePath("OpenFL Test/test.txt");
        myFileStream = new FileStream();
        myFileStream.addEventListener(ProgressEvent.PROGRESS, progressHandler);
        myFileStream.openAsync(myFile, FileMode.READ);
    }

    private function progressHandler(event:ProgressEvent):Void
    {
        myFileStream.readBytes(bytes, myFileStream.position, myFileStream.bytesAvailable);
    }
}
```

For a file opened asynchronously, only the data in the read buffer can be read.
Furthermore, as you read the data, it is removed from the read buffer. For read
operations, you need to ensure that the data exists in the read buffer before
calling the read operation. For example, the following code reads 8000 bytes of
data starting from position 4000 in the file:

```haxe
import openfl.display.Sprite;
import openfl.events.ProgressEvent;
import openfl.filesystem.File;
import openfl.filesystem.FileMode;
import openfl.filesystem.FileStream;
import openfl.utils.ByteArray;

class FileStreamReadBufferExample3 extends Sprite
{
    private var bytes:ByteArray;
    private var myFileStream:FileStream;

    public function new()
    {
        super();

        bytes = new ByteArray();
        var myFile:File = File.documentsDirectory.resolvePath("OpenFL Test/test.txt");
        myFileStream = new FileStream();
        myFileStream.addEventListener(ProgressEvent.PROGRESS, progressHandler);
        myFileStream.openAsync(myFile, FileMode.READ);
        myFileStream.position = 4000;
    }

    private function progressHandler(event:ProgressEvent):Void
    {
        if (myFileStream.bytesAvailable > 8000 )
        {
            myFileStream.readBytes(bytes, myFileStream.position, 8000);
        }
    }
}
```

During a write operation, the FileStream object does not read data into the read
buffer. When a write operation completes (all data in the write buffer is
written to the file), the FileStream object starts a new read buffer (assuming
that the associated FileStream object was opened with read capabilities), and
starts reading data into the read buffer, starting from the position specified
by the `position` property. The `position` property may be the position of the
last byte written, or it may be a different position, if the user specifies a
different value for the `position` object after the write operation.
