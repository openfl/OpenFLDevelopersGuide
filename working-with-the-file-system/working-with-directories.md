# Working with directories

OpenFL provides you with capabilities to work with directories on the local file
system.

For details on creating
[File objects](https://api.openfl.org/openfl/filesystem/File.html) that point to
directories, see
[Pointing a File object to a directory](./working-with-file-objects-in-openfl.md#pointing-a-file-object-to-a-directory).

## Creating directories

The `File.createDirectory()` method lets you create a directory. For example,
the following code creates a directory named _OpenFL Test_ as a subdirectory of
the user's home directory:

```haxe
import openfl.display.Sprite;
import openfl.filesystem.File;

class CreateDirectoryExample extends Sprite
{
    public function new()
    {
        super();

        var dir:File = File.userDirectory.resolvePath("OpenFL Test");
        dir.createDirectory();
    }
}
```

If the directory exists, the `createDirectory()` method does nothing.

Also, in some modes, a FileStream object creates directories when opening files.
Missing directories are created when you instantiate a FileStream instance with
the `fileMode` parameter of the `FileStream()` constructor set to
`FileMode.APPEND` or `FileMode.WRITE`. For more information, see
[Workflow for reading and writing files](./workflow-for-reading-and-writing-files.md).

## Creating a temporary directory

The File class includes a `createTempDirectory()` method, which creates a
directory in the temporary directory folder for the System, as in the following
example:

```haxe
import openfl.display.Sprite;
import openfl.filesystem.File;

class CreateTempDirectoryExample extends Sprite
{
    public function new()
    {
        super();

        var temp:File = File.createTempDirectory();
    }
}
```

The `createTempDirectory()` method automatically creates a unique temporary
directory (saving you the work of determining a new unique location).

You can use a temporary directory to temporarily store temporary files used for
a session of the application. Note that there is a `createTempFile()` method for
creating new, unique temporary files in the System temporary directory.

You may want to delete the temporary directory before closing the application,
as it is _not_ automatically deleted on all devices.

## Enumerating directories

You can use the `getDirectoryListing()` method or the
`getDirectoryListingAsync()` method of a File object to get an array of File
objects pointing to files and subfolders in a directory.

For example, the following code lists the contents of the user's documents
directory (without examining subdirectories):

```haxe
import openfl.display.Sprite;
import openfl.filesystem.File;

class GetDirectoryListingExample extends Sprite
{
    public function new()
    {
        super();

        var directory:File = File.documentsDirectory;
        var contents:Array<File> = directory.getDirectoryListing();
        for (i in 0...contents.length)
        {
            trace(contents[i].name, contents[i].size);
        }
    }
}
```

When using the asynchronous version of the method, the `directoryListing` event
object has a `files` property that is the array of File objects pertaining to
the directories:

```haxe
import openfl.display.Sprite;
import openfl.events.FileListEvent;
import openfl.filesystem.File;

class GetDirectoryListingAsyncExample extends Sprite
{
    public function new()
    {
        super();

        var directory:File = File.documentsDirectory;
        directory.addEventListener(FileListEvent.DIRECTORY_LISTING, dirListHandler);
        directory.getDirectoryListingAsync();
    }

    private function dirListHandler(event:FileListEvent):Void
    {
        var contents:Array<File> = event.files;
        for (i in 0...contents.length)
        {
            trace(contents[i].name, contents[i].size);
        }
    }
}
```

## Copying and moving directories

You can copy or move a directory, using the same methods as you would to copy or
move a file. For example, the following code copies a directory synchronously:

```haxe
import openfl.display.Sprite;
import openfl.filesystem.File;

class DirectoryCopyToExample extends Sprite
{
    public function new()
    {
        super();

        var sourceDir:File = File.documentsDirectory.resolvePath("OpenFL Test");
        var resultDir:File = File.documentsDirectory.resolvePath("OpenFL Test Copy");
        sourceDir.copyTo(resultDir);
    }
}
```

When you specify true for the `overwrite` parameter of the `copyTo()` method,
all files and folders in an existing target directory are deleted and replaced
with the files and folders in the source directory (even if the target file does
not exist in the source directory).

The directory that you specify as the `newLocation` parameter of the `copyTo()`
method specifies the path to the resulting directory; it does _not_ specify the
_parent_ directory that will contain the resulting directory.

For details, see
[Copying and moving files](./working-with-files.md#copying-and-moving-files).

## Deleting directory contents

The File class includes a `deleteDirectory()` method and a
`deleteDirectoryAsync()` method. These methods delete directories, the first
working synchronously, the second working asynchronously (see
[Native file system basics](./native-file-system-basics.md)). Both methods include a
`deleteDirectoryContents` parameter (which takes a Boolean value); when this
parameter is set to `true` (the default value is `false`) the call to the method
deletes non-empty directories; otherwise, only empty directories are deleted.

For example, the following code synchronously deletes the _OpenFL Test_
subdirectory of the user's documents directory:

```haxe
import openfl.display.Sprite;
import openfl.filesystem.File;

class DeleteDirectoryExample extends Sprite
{
    public function new()
    {
        super();

        var directory:File = File.documentsDirectory.resolvePath("OpenFL Test");
        directory.deleteDirectory(true);
    }
}
```

The following code asynchronously deletes the _OpenFL Test_ subdirectory of the
user's documents directory:

```haxe
import openfl.display.Sprite;
import openfl.filesystem.File;

class DeleteDirectoryAsyncExample extends Sprite
{
    public function new()
    {
        super();

        var directory:File = File.documentsDirectory.resolvePath("OpenFL Test");
        directory.addEventListener(Event.COMPLETE, completeHandler)
        directory.deleteDirectoryAsync(true);
    }

    private function completeHandler(event:Event):Void
    {
        trace("Deleted.")
    }
}
```

<!-- TODO: uncomment when moveToTrash() is implemented in OpenFL
Also included are the `moveToTrash()` and `moveToTrashAsync()` methods, which
you can use to move a directory to the System trash. For details, see
[Moving a file to the trash](./working-with-files.md#moving-a-file-to-the-trash).
-->
