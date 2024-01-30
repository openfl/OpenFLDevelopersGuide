# Using the load() and save() methods

The `load()` and `save()` methods aree available on the FileReference class, and
the File class inherits them. These methods were designed to provide a secure
means for users to load and save file data in web browsers. However, OpenFL
applications targeting native platforms can also use these methods as an easy
way to load and save files asynchronously.

For example, the following code saves a string to a text file:

```haxe
import openfl.display.Sprite;
import openfl.events.Event;
import openfl.filesystem.File;

class FileSaveExample extends Sprite
{
    private var file:File;

    public function new()
    {
        super();

        file = File.applicationStorageDirectory.resolvePath("test.txt");
        var str:String = "Hello.";
        file.addEventListener(Event.COMPLETE, fileSaved);
        file.save(str);
    }

    private function fileSaved(event:Event):Void
    {
        trace("Done.");
    }
}
```

The `data` parameter of the `save()` method can take a String, XML, or ByteArray
value. When the argument is a String or XML value, the method saves the file as
a UTF-8–encoded text file.

When this code sample executes, the application displays a dialog box in which
the user selects the saved file destination.

The following code loads a string from a UTF-8–encoded text file:

```haxe
import openfl.display.Sprite;
import openfl.events.Event;
import openfl.filesystem.File;
import openfl.utils.ByteArray;

class FileLoadExample extends Sprite
{
    private var file:File;
    private var str:String;

    public function new()
    {
        super();

        file = File.applicationStorageDirectory.resolvePath("test.txt");
        file.addEventListener(Event.COMPLETE, loaded);
        file.load();
    }

    private function loaded(event:Event):Void
    {
        var bytes:ByteArray = file.data;
        str = bytes.readUTFBytes(bytes.length);
        trace(str);
    }
}
```

The FileStream class provides more functionality than that provided by the
`load()` and `save()` methods:

- Using the FileStream class, you can read and write data both synchronously and
  asynchronously.

- Using the FileStream class lets you write incrementally to a file.

- Using the FileStream class lets you open a file for random access (both
  reading from and writing to any section of the file).

- The FileStream class lets you specify the type of file access you have to the
  file, by setting the `fileMode` parameter of the `open()` or `openAsync()`
  method.

- The FileStream class lets you save data to files without presenting the user
  with an Open or Save dialog box.

- You can directly use types other than byte arrays when reading data with the
  FileStream class.
