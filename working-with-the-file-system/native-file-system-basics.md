# Native file system basics

OpenFL provides classes that you can use to access, create, and manage both
files and folders. These classes, contained in the
[openfl.filesystem package](https://api.openfl.org/openfl/filesystem/index.html),
are used as follows:

| File classes                                                       | Description                                                                                                                                                                                                                                                                                                                                |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [File](https://api.openfl.org/openfl/filesystem/File.html)         | File object represents a path to a file or directory. You use a file object to create a pointer to a file or folder, initiating interaction with the file or folder.                                                                                                                                                                       |
| [FileMode](https://api.openfl.org/openfl/filesystem/FileMode.html) | The FileMode class defines string constants used in the `fileMode` parameter of the `open()` and `openAsync()` methods of the FileStream class. The `fileMode` parameter of these methods determines the capabilities available to the FileStream object once the file is opened, which include writing, reading, appending, and updating. |
| [FileStream](api.openfl.org/openfl/filesystem/FileStream.html)     | FileStream object is used to open files for reading and writing. Once you've created a File object that points to a new or existing file, you pass that pointer to the FileStream object so that you can open it and read or write data.                                                                                                   |

Some methods in the File class have both synchronous and asynchronous versions:

- `File.copyTo()` and `File.copyToAsync()`

- `File.deleteDirectory()` and `File.deleteDirectoryAsync()`

- `File.deleteFile()` and `File.deleteFileAsync()`

- `File.getDirectoryListing()` and `File.getDirectoryListingAsync()`

- `File.moveTo()` and `File.moveToAsync()`

- `File.moveToTrash()` and `File.moveToTrashAsync()`

Also, FileStream operations work synchronously or asynchronously depending on
how the FileStream object opens the file: by calling the `open()` method or by
calling the `openAsync()` method.

The asynchronous versions let you initiate processes that run in the background
and dispatch events when complete (or when error events occur). Other code can
execute while these asynchronous background processes are taking place. With
asynchronous versions of the operations, you must set up event listener
functions, using the `addEventListener()` method of the File or FileStream
object that calls the function.

The synchronous versions let you write simpler code that does not rely on
setting up event listeners. However, since other code cannot execute while a
synchronous method is executing, important processes such as display object
rendering and animation can be delayed.
