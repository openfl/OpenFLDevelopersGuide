# Working with File objects in OpenFL

A [File object](https://api.openfl.org/openfl/filesystem/File.html) is a pointer
to a file or directory in the file system.

The File class extends the FileReference class. The FileReference class, which
is available in web browsers as well as in native applications, represents a
pointer to a file. The File class adds properties and methods that are not
exposed in web browsers, due to security considerations.

## About the File class

You can use the File class for the following:

- Getting the path to special directories, including the user directory, the
  user's documents directory, the directory from which the application was
  launched, and the application directory

- Coping files and directories

- Moving files and directories

- Deleting files and directories (or moving them to the trash)

- Listing files and directories contained in a directory

- Creating temporary files and folders

Once a File object points to a file path, you can use it to read and write file
data, using the FileStream class.

A File object can point to the path of a file or directory that does not yet
exist. You can use such a File object in creating a file or directory.

## Paths of File objects

Each File object has two properties that each define its path:

| Property     | Description                                                                                                                                                                                                                                                                                                                                                 |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `nativePath` | Specifies the platform-specific path to a file. For example, on Windows a path might be "c:\Sample directory\test.txt" whereas on macOS it could be "/Sample directory/test.txt". A `nativePath` property uses the backslash (\\ character as the directory separator character on Windows, and it uses the forward slash (/) character on macOS and Linux. |
| `url`        | This may use the file URL scheme to point to a file. For example, on Windows a path might be "file:///c:/Sample%20directory/test.txt" whereas on macOS it could be "file:///Sample%20directory/test.txt". The runtime includes other special URL schemes besides `file` and are described in [Supported OpenFL URL schemes](#supported-openfl-url-schemes)  |

The File class includes static properties for pointing to standard directories
on macOS, Windows, and Linux. These properties include:

- `File.applicationStorageDirectory` — a storage directory unique to each
  installed OpenFL application. This directory is an appropriate place to store
  dynamic application assets and user preferences. Consider storing large
  amounts of data elsewhere.

  On Android and iOS, the application storage directory is removed when the
  application is uninstalled or the user chooses to clear application data, but
  this is not the case on other platforms.

- `File.applicationDirectory` — the directory where the application is installed
  (along with any installed assets). On some operating systems, the application
  is stored in a single package file rather than a physical directory. In this
  case, the contents may not be accessible using the native path. The
  application directory is read-only.

- `File.desktopDirectory` — the user's desktop directory. If a platform does not
  define a desktop directory, another location on the file system is used.

- `File.documentsDirectory` — the user's documents directory. If a platform does
  not define a documents directory, another location on the file system is used.

- `File.userDirectory` — the user directory. If a platform does not define a
  user directory, another location on the file system is used.

Note: When a platform does not define standard locations for desktop, documents,
or user directories, `File.documentsDirectory`, `File.desktopDirectory`, and
`File.userDirectory` may reference the same directory.

These properties have different values on different operating systems. For
example, macOS and Windows each have a different native path to the user's
desktop directory. However, the `File.desktopDirectory` property points to an
appropriate directory path on every platform. To write applications that work
well across platforms, use these properties as the basis for referencing other
directories and files used by the application. Then use the `resolvePath()`
method to refine the path. For example, this code points to the preferences.xml
file in the application storage directory:

```haxe
var prefsFile:File = File.applicationStorageDirectory;
prefsFile = prefsFile.resolvePath("preferences.xml");
```

Although the File class lets you point to a specific file path, doing so can
lead to applications that do not work across platforms. For example, the path
_C:\Users\joe\\_ only works on Windows. For these reasons, it is best to use the
static properties of the File class, such as `File.documentsDirectory`.

### Common directory locations

<table>
<thead>
    <tr>
        <th><p>Platform</p></th>
        <th><p>Directory type</p></th>
        <th><p>Typical file system location</p></th>
    </tr>
</thead>
<tbody>
    <tr>
        <td rowspan="6"><p>Linux</p></td>
        <td><p>Application</p></td>
        <td><p><samp>/opt/</samp><em><samp>&lt;Program Name&gt;</samp></em><samp>/share</samp></p></td>
    </tr>
    <tr>
        <td><p>Application-storage</p></td>
        <td><p><samp>/home/</samp><em><samp>&lt;userName&gt;</samp></em><samp>/.local/share/</samp><samp><em>&lt;Program Name&gt;</samp></p></td>
    </tr>
    <tr>
        <td><p>Desktop</p></td>
        <td><p><samp>/home/</samp><em><samp>&lt;userName&gt;</samp></em><samp>/Desktop</samp></p></td>
    </tr>
    <tr>
        <td><p>Documents</p></td>
        <td><p><samp>/home/</samp><em><samp>&lt;userName&gt;</samp></em><samp>/Documents</samp></p></td>
    </tr>
    <tr>
        <td><p>Temporary</p></td>
        <td><p><samp>/tmp/ofl-</samp><em><samp>&lt;randomString&gt;</samp></em><samp>.tmp</samp></p></td>
    </tr>
    <tr>
        <td><p>User</p></td>
        <td><p><samp>/home/</samp><em><samp>&lt;userName&gt;</samp></em></p></td>
    </tr>
    <tr>
        <td rowspan="6"><p>macOS</p></td>
        <td><p>Application</p></td>
        <td><p><samp>/Applications/</samp><em><samp>&lt;Program Name&gt;</samp></em><samp>.app/Contents/Resources</samp></p></td>
    </tr>
    <tr>
        <td><p>Application-storage</p></td>
        <td><p><samp>/Users/</samp><em><samp>&lt;userName&gt;</samp></em><samp>/Library/Application Support/</samp><em><samp>&lt;Program Name&gt;</samp></em></p></td>
    </tr>
    <tr>
        <td><p>Desktop</p></td>
        <td><p><samp>/Users/</samp><em><samp>&lt;userName&gt;</samp></em><samp>/Desktop</samp></p></td>
    </tr>
    <tr>
        <td><p>Documents</p></td>
        <td><p><samp>/Users/</samp><em><samp>&lt;userName&gt;</samp></em><samp>/Documents</samp></p></td>
    </tr>
    <tr>
        <td><p>Temporary</p></td>
        <td><p><samp>/var/folders/ofl-</samp><em><samp>&lt;randomString&gt;</samp></em><samp>.tmp</samp></p></td>
    </tr>
    <tr>
        <td><p>User</p></td>
        <td><p><samp>/Users/</samp><em><samp>&lt;userName&gt;</samp></em></p></td>
    </tr>
    <tr>
        <td rowspan="6"><p>Windows</p></td>
        <td><p>Application</p></td>
        <td><p><samp>C:\Program Files\</samp><em><samp>&lt;Program Name&gt;</samp></em></p></td>
    </tr>
    <tr>
        <td><p>Application-storage</p></td>
        <td><p><samp>C:\Users\</samp><em><samp>&lt;userName&gt;</samp></em><samp>\AppData\Roaming\</samp><em><samp>&lt;Company Name&gt;</samp></em><samp>\</samp><em><samp>&lt;Program Name&gt;</samp></em></p></td>
    </tr>
    <tr>
        <td><p>Desktop</p></td>
        <td><p><samp>C:\Users\</samp><em><samp>&lt;userName&gt;</samp></em><samp>\Desktop</samp></p></td>
    </tr>
    <tr>
        <td><p>Documents</p></td>
        <td><p><samp>C:\Users\</samp><em><samp>&lt;userName&gt;</samp></em><samp>\Documents</samp></p></td>
    </tr>
    <tr>
        <td><p>Temporary</p></td>
        <td><p><samp>C:\Users\</samp><em><samp>&lt;userName&gt;</samp></em><samp>\AppData\Local\Temp\ofl-</samp><em><samp>&lt;randomString&gt;</samp></em><samp>.tmp</samp></p></td>
    </tr>
    <tr>
        <td><p>User</p></td>
        <td><p><samp>C:\Users\</samp><em><samp>&lt;userName&gt;</samp></em></p></td>
    </tr>
</tbody>
</table>

The actual native paths for these directories vary based on the operating system
and computer configuration. The paths shown in this table are typical examples.
You should always use the appropriate static File class properties to refer to
these directories so that your application works correctly on any platform. In
an actual OpenFL application, the values in angle brackets, like
`<Program Name>` and `<userName>`, are taken either from the operating system or
the OpenFL _project.xml_ file.

## Pointing a File object to a directory

There are different ways to set a File object to point to a directory.

### Pointing to the user's home directory

You can point a File object to the user's home directory. The following code
sets a File object to point to an _OpenFL Test_ subdirectory of the home
directory:

```haxe
var file:File = File.userDirectory.resolvePath("OpenFL Test");
```

### Pointing to the user's documents directory

You can point a File object to the user's documents directory. The following
code sets a File object to point to an _OpenFL Test_ subdirectory of the
documents directory:

```haxe
var file:File = File.documentsDirectory.resolvePath("OpenFL Test");
```

### Pointing to the desktop directory

You can point a File object to the desktop. The following code sets a File
object to point to an _OpenFL Test_ subdirectory of the desktop:

```haxe
var file:File = File.desktopDirectory.resolvePath("OpenFL Test");
```

### Pointing to the application storage directory

You can point a File object to the application storage directory. For every
OpenFL application, there is a unique associated path that defines the
application storage directory. This directory is unique to each application and
user. You can use this directory to store user-specific, application-specific
data (such as user data or preferences files). For example, the following code
points a File object to a preferences file, prefs.xml, contained in the
application storage directory:

```haxe
var file:File = File.applicationStorageDirectory;
file = file.resolvePath("prefs.xml");
```

The application storage directory location is typically based on the user name
and the application ID. The following file system locations are given here to
help you debug your application. You should always use the
`File.applicationStorage` property or `app-storage:` URI scheme to resolve files
in this directory:

- On macOS — In the user home directory:

  <samp>/Users/</samp><em><samp>&lt;userName&gt;</samp></em><samp>/Library/Application
  Support/</samp><em><samp>&lt;Program Name&gt;</samp></em>

  For example:

      /Users/babbage/Library/Application Support/My Test App

- On Windows — In the user home directory, in:

  <samp>C:\Users\</samp><em><samp>&lt;userName&gt;</samp></em><samp>\AppData\Roaming\</samp><em><samp>&lt;Company
  Name&gt;</samp></em><samp>\</samp><em><samp>&lt;Program
  Name&gt;</samp></em></p>

  For example:

      C:\Users\babbage\Application Data\My Company\My Test App

- On Linux — In the user home directory:

  <samp>/home/</samp><em><samp>&lt;userName&gt;</samp></em><samp>/.local/share/</samp><samp><em>&lt;Program
  Name&gt;</samp>

  For example:

      /home/babbage/.local/share/My Test App

Note: If an application has a publisher ID, then the publisher ID is also used
as part of the path to the application storage directory.

<!-- TODO: uncomment when File.url is implemented
The URL (and `url` property) for a File object created with
`File.applicationStorageDirectory` uses the `app-storage` URL scheme (see
[Supported OpenFL URL schemes](#supported-openfl-url-schemes)), as in the
following:

```haxe
var dir:File = File.applicationStorageDirectory;
dir = dir.resolvePath("preferences");
trace(dir.url); // app-storage:/preferences
```
-->

### Pointing to the application directory

You can point a File object to the directory in which the application was
installed, known as the application directory. You can reference this directory
using the `File.applicationDirectory` property. You can use this directory to
examine the application descriptor file or other resources installed with the
application. For example, the following code points a File object to a directory
named _images_ in the application directory:

```haxe
var dir:File = File.applicationDirectory;
dir = dir.resolvePath("images");
```

<!--TODO: uncomment when File.url is implemented
The URL (and `url` property) for a File object created with
`File.applicationDirectory` uses the `app` URL scheme (see
[Supported OpenFL URL schemes](#supported-openfl-url-schemes)), as in the
following:

```haxe
var dir:File = File.applicationDirectory;
dir = dir.resolvePath("images");
trace(dir.url); // app:/images
```

Note: On Android, the files in the application package are not accessible via
the `nativePath`. The `nativePath` property is an empty string. Always use the
URL to access files in the application directory rather than a native path.
-->

<!-- TODO: uncomment if cacheDirectory is implemented
### Pointing to the cache directory

You can point a File object to the operating system's temporary or cache
directory using the `File.cacheDirectory` property. This directory contains
temporary files that are not required for the application to run and will not
cause problems or data loss for the user if they are deleted.

In most operating systems the cache directory is a temporary directory. On iOS,
the cache directory corresponds to the application library's Caches directory.
Files in this directory are not backed up to online storage, and can potentially
be deleted by the operating system if the device's available storage space is
too low. For more information, see
[Controlling file backup and caching](#controlling-file-backup-and-caching).-->

### Pointing to the file system root

The `File.getRootDirectories()` method lists all root volumes, such as C: and
mounted volumes, on a Windows computer. On macOS and Linux, this method always
returns the unique root directory for the machine (the "/" directory).

<!-- TODO: uncommnt if StorageVolumeInfo is implemented
The `StorageVolumeInfo.getStorageVolumes()` method provides more detailed
information on mounted storage volumes (see
[Working with storage volumes](./working-with-storage-volumes.md)).-->

Note: The root of the file system is not readable on Android. A File object
referencing the directory with the native path, "/", is returned, but the
properties of that object do not have accurate values. For example,
`spaceAvailable` is always 0.

### Pointing to an explicit directory

You can point the File object to an explicit directory by setting the
`nativePath` property of the File object, as in the following example (on
Windows):

```haxe
var file:File = new File();
file.nativePath = "C:\\OpenFL Test";
```

**Important:** Pointing to an explicit path this way can lead to code that does
not work across platforms. For example, the previous example only works on
Windows. You can use the static properties of the File object, such as
`File.applicationStorageDirectory`, to locate a directory that works
cross-platform. Then use the `resolvePath()` method (see the next section) to
navigate to a relative path.

### Navigating to relative paths

You can use the `resolvePath()` method to obtain a path relative to another
given path. For example, the following code sets a File object to point to an
_OpenFL Test_ subdirectory of the user's home directory:

```haxe
var file:File = File.userDirectory;
file = file.resolvePath("OpenFL Test");
```

<!-- TODO: uncomment when File.url is implemented
You can also use the `url` property of a File object to point it to a directory
based on a URL string, as in the following:

```haxe
var urlStr:String = "file:///C:/OpenFL Test/";
var file:File = new File()
file.url = urlStr;
```
-->

For more information, see [Modifying File paths](#modifying-file-paths).

### Letting the user browse to select a directory

The File class includes the `browseForDirectory()` method, which presents a
system dialog box in which the user can select a directory to assign to the
object. The `browseForDirectory()` method is asynchronous. The File object
dispatches a `select` event if the user selects a directory and clicks the Open
button, or it dispatches a `cancel` event if the user clicks the Cancel button.

For example, the following code lets the user select a directory and outputs the
directory path upon selection:

```haxe
import openfl.display.Sprite;
import openfl.events.Event;
import openfl.filesystem.File;

class FileBrowseForDirectoryExample extends Sprite
{
    private var file:File;

    public function new()
    {
        super();

        file = new File();
        file.addEventListener(Event.SELECT, dirSelected);
        file.browseForDirectory("Select a directory");
    }

    private function dirSelected(e:Event):Void
    {
        trace(file.nativePath);
    }
}
```

Note: On Android, the `browseForDirectory()` method is not supported. Calling
this method has no effect; a cancel event is dispatched immediately. To allow
users to select a directory, use a custom, application-defined dialog, instead.

### Pointing to the directory from which the application was invoked

You can get the directory location from which an application is invoked, by
checking the `currentDirectory` property of the InvokeEvent object dispatched
when the application is invoked.

<!-- uncomment when this content is converted to OpenFL
For details, see [Capturing command line arguments](../../client-system-interaction/air-application-invokation-and-termination.md#capturing-command-line-arguments).-->

## Pointing a File object to a file

There are different ways to set the file to which a File object points.

### Pointing to an explicit file path

**Important:** Pointing to an explicit path can lead to code that does not work
across platforms. For example, the path C:/foo.txt only works on Windows. You
can use the static properties of the File object, such as
`File.applicationStorageDirectory`, to locate a directory that works
cross-platform. Then use the `resolvePath()` method (see
[Modifying File paths](#modifying-file-paths)) to navigate to a relative path.

<!-- TODO: uncomment when File.url is implemented
You can use the `url` property of a File object to point it to a file or
directory based on a URL string, as in the following:

```haxe
var urlStr:String = "file:///C:/OpenFL Test/test.txt";
var file:File = new File()
file.url = urlStr;
```

You can also pass the URL to the `File()` constructor function, as in the
following:

```haxe
var urlStr:String = "file:///C:/OpenFL Test/test.txt";
var file:File = new File(urlStr);
```

The `url` property always returns the URI-encoded version of the URL (for
example, blank spaces are replaced with `"%20`):

```haxe
file.url = "file:///c:/OpenFL Test";
trace(file.url); // file:///c:/OpenFL%20Test
```
-->

You can also use the `nativePath` property of a File object to set an explicit
path. For example, the following code, when run on a Windows computer, sets a
File object to the test.txt file in the _OpenFL Test_ subdirectory of the C:
drive:

```haxe
var file:File = new File();
file.nativePath = "C:/OpenFL Test/test.txt";
```

You can also pass this path to the `File()` constructor function, as in the
following:

```haxe
var file:File = new File("C:/OpenFL Test/test.txt");
```

Use the forward slash (/) character as the path delimiter for the `nativePath`
property. On Windows, you can also use the backslash (\\ character, but doing so
leads to applications that do not work across platforms.

For more information, see [Modifying File paths](#modifying-file-paths).

### Enumerating files in a directory

You can use the `getDirectoryListing()` method of a File object to get an array
of File objects pointing to files and subdirectories at the root level of a
directory. For more information, see
[Enumerating directories](./working-with-directories.md#enumerating-directories).

### Letting the user browse to select a file

The File class includes the following methods that present a system dialog box
in which the user can select a file to assign to the object:

- `browseForOpen()`

- `browseForSave()`

- `browseForOpenMultiple()`

These methods are each asynchronous. The `browseForOpen()` and `browseForSave()`
methods dispatch the select event when the user selects a file (or a target
path, in the case of browseForSave()). With the `browseForOpen()` and
`browseForSave()` methods, upon selection the target File object points to the
selected files. The `browseForOpenMultiple` () method dispatches a
`selectMultiple` event when the user selects files. The `selectMultiple` event
is of type FileListEvent, which has a `files` property that is an array of File
objects (pointing to the selected files).

For example, the following code presents the user with an "Open" dialog box in
which the user can select a file:

```haxe
import openfl.display.Sprite;
import openfl.events.Event;
import openfl.filesystem.File;
import openfl.net.FileFilter;

class FileBrowseForOpenExample extends Sprite
{
    private var fileToOpen:File;

    public function new()
    {
        super();

        fileToOpen = File.documentsDirectory;
        selectTextFile(fileToOpen);
    }

    private function selectTextFile(root:File):Void
    {
        var txtFilter:FileFilter = new FileFilter("Text", "*.hx;*.css;*.html;*.txt;*.xml");
        root.browseForOpen("Open", [txtFilter]);
        root.addEventListener(Event.SELECT, fileSelected);
    }

    private function fileSelected(event:Event):Void
    {
        trace(fileToOpen.nativePath);
    }
}
```

If the application has another browser dialog box open when you call a browse
method, the runtime throws an Error exception.

Note: On Android, only image, video, and audio files can be selected with the
`browseForOpen()` and `browseForOpenMultiple()` methods. The browseForSave()
dialog also displays only media files even though the user can enter an
arbitrary filename. For opening and saving non-media files, you should consider
using custom dialogs instead of these methods.

## Modifying File paths

You can also modify the path of an existing File object by calling the
`resolvePath()` method or by modifying the `nativePath` or `url` property of the
object, as in the following examples (on Windows):

```haxe
import openfl.display.Sprite;
import openfl.filesystem.File;

class ModifyFilePathExample extends Sprite
{
    public function new()
    {
        super();

        var file1:File = File.documentsDirectory;
        file1 = file1.resolvePath("OpenFL Test");
        trace(file1.nativePath); // C:\Users\userName\My Documents\OpenFL Test
        var file2:File = File.documentsDirectory;
        file2 = file2.resolvePath("..");
        trace(file2.nativePath); // C:\Users\userName
        var file3:File = File.documentsDirectory;
        file3.nativePath += "/subdirectory";
        trace(file3.nativePath); // C:\Users\userName\My Documents\subdirectory
    }
}
```

<!-- TODO: add to example above when File.url is implemented
        var file4:File = new File();
        file4.url = "file:///c:/OpenFL Test/test.txt";
        trace(file4.nativePath); // C:\OpenFL Test\test.txt
-->

When using the `nativePath` property, use the forward slash (/) character as the
directory separator character. On Windows, you can use the backslash (\\
character as well, but you should not do so, as it leads to code that does not
work cross-platform.

## Supported OpenFL URL schemes

In OpenFL, you can use any of the following URL schemes in defining the `url`
property of a File object:

<table>
<thead>
    <tr>
        <th><p>URL scheme</p></th>
        <th><p>Description</p></th>
    </tr>
</thead>
<tbody>
    <tr>
        <td><p>file</p></td>
        <td><p>Use to specify a path relative to the root of the file system.
        For example:</p><pre><code>file:///c:/OpenFL Test/test.txt</code></pre><p>The URL standard specifies that a file URL takes the form
        <samp>file://&lt;host&gt;/&lt;path&gt;</samp>. As a special case,
        <samp>&lt;host&gt;</samp> can be the empty string, which is interpreted
        as "the machine from which the URL is being interpreted." For this
        reason, file URLs often have three slashes (///).</p></td>
    </tr>
    <tr>
        <td><p>app</p></td>
        <td><p><em>(Since OpenFL 9.4)</em> Use to specify a path relative to the root directory of the
        installed application (the directory that contains the application.xml
        file for the installed application). For example, the following path
        points to an images subdirectory of the directory of the installed
        application:</p><pre><code>app:/images</code></pre>
        </div></td>
    </tr>
    <tr>
        <td><p>app-storage</p></td>
        <td><p><em>(Since OpenFL 9.4)</em> Use to specify a path relative to the application store
        directory. For each installed application, OpenFL defines a unique
        application store directory, which is a useful place to store data
        specific to that application. For example, the following path points to
        a prefs.xml file in a settings subdirectory of the application store
        directory:</p><pre><code>app-storage:/settings/prefs.xml</code></pre>
        </div></td>
    </tr>
</tbody>
</table>

<!-- TODO: uncomment if implemented in OpenFL
## Controlling file backup and caching

Certain operating systems, most notably iOS and macOS provide users the ability
to automatically back up application files to a remote storage. In addition, on
iOS there are restrictions on whether files can be backed up and also where
files of different purposes can be stored.

The following summarize how to comply with Apple's guidelines for file backup
and storage. For further information see the next sections.

- To specify that a file does not need to be backed up and (iOS only) can be
  deleted by the operating system if device storage space runs low, save the
  file in the cache directory (`File.cacheDirectory`). This is the preferred
  storage location on iOS and should be used for most files that can be
  regenerated or re-downloaded.

- To specify that a file does not need to be backed up, but should not be
  deleted by the operating system, save the file in one of the application
  library directories such as the application storage directory (
  `File.applicationStorageDirectory`) or the documents directory (
  `File.documentsDirectory`). Set the File object's `preventBackup` property to
  `true`. This is required by Apple for content that can be regenerated or
  downloaded again, but which is required for proper functioning of your
  application during offline use.

#### Specifying files for backup

In order to save backup space and reduce network bandwidth use, Apple's
guidelines for iOS and Mac applications specify that only files that contain
user-entered data or data that otherwise can't be regenerated or re-downloaded
should be designated for backup.

By default all files in the application library folders are backed up. On macOS
this is the application storage directory. On iOS, this includes the
application storage directory, the application directory, the desktop directory,
documents directory, and user directory (because those directories are mapped to
application library folders on iOS). Consequently, any files in those
directories are backed up to server storage by default.

If you are saving a file in one of those locations that can be re-created by
your application, you should flag the file so the operating system knows not to
back it up. To indicate that a file should not be backed up, set the File
object's `preventBackup` property to `true`.

Note that on iOS, for a file in any of the application library folders, even if
the file's `preventBackup` property is set to `true` the file is flagged as a
persistent file that the operating system shouldn't delete.

#### Controlling file caching and deletion

Apple's guidelines for iOS applications specify that as much as possible,
content that can be regenerated should be made available to the operating system
to delete in case the device runs low on storage space.

On iOS, files in the application library folders (such as the application
storage directory or the documents directory) are flagged as permanent and are
not deleted by the operating system.

Save files that can be regenerated by the application and are safe to delete in
case of low storage space in the application cache directory. You access the
cache directory using the `File.cacheDirectory` static property.

On iOS the cache directory corresponds to the application's cache directory
(\<Application Home\>/Library/Caches). On other operating systems, this
directory is mapped to a comparable directory. For example, on macOS it also
maps to the Caches directory in the application library. On Android the cache
directory maps to the application's cache directory. On Windows, the cache
directory maps to the operating system temp directory. On both Android and
Windows, this is the same directory that is accessed by a call to the File
class's `createTempDirectory()` and `createTempFile()` methods.-->

## Finding the relative path between two files

You can use the `getRelativePath()` method to find the relative path between two
files:

```haxe
import openfl.display.Sprite;
import openfl.filesystem.File;

class FileGetRelativePathExample1 extends Sprite
{
    public function new()
    {
        super();

        var file1:File = File.documentsDirectory.resolvePath("OpenFL Test");
        var file2:File = File.documentsDirectory;
        file2 = file2.resolvePath("OpenFL Test/bob/test.txt");

        trace(file1.getRelativePath(file2)); // bob/test.txt
    }
}
```

The second parameter of the `getRelativePath()` method, the `useDotDot`
parameter, allows for `..` syntax to be returned in results, to indicate parent
directories:

```haxe
import openfl.display.Sprite;
import openfl.filesystem.File;

class FileGetRelativePathExample2 extends Sprite
{
    public function new()
    {
        super();

        var file1:File = File.documentsDirectory;
        file1 = file1.resolvePath("OpenFL Test");
        var file2:File = File.documentsDirectory;
        file2 = file2.resolvePath("OpenFL Test/bob/test.txt");
        var file3:File = File.documentsDirectory;
        file3 = file3.resolvePath("OpenFL Test/susan/test.txt");

        trace(file2.getRelativePath(file1, true)); // ../..
        trace(file3.getRelativePath(file2, true)); // ../../bob/test.txt
    }
}
```

## Obtaining canonical versions of file names

File and path names are not case sensitive on Windows and macOS. In the
following, two File objects point to the same file:

```haxe
File.documentsDirectory.resolvePath("test.txt");
File.documentsDirectory.resolvePath("TeSt.TxT");
```

However, documents and directory names do include capitalization. For example,
the following assumes that there is a folder named _OpenFL Test_ in the
documents directory, as in the following examples:

```haxe
import openfl.display.Sprite;
import openfl.filesystem.File;

class FileCanonicalizeExample1 extends Sprite
{
    public function new()
    {
        super();

        var file:File = File.documentsDirectory.resolvePath("OpenFL test");
        trace(file.nativePath); // ... OpenFL test
        file.canonicalize();
        trace(file.nativePath); // ... OpenFL Test
    }
}
```

The `canonicalize()` method converts the `nativePath` object to use the correct
capitalization for the file or directory name. On case sensitive file systems
(such as Linux), when multiple files exists with names differing only in case,
the `canonicalize()` method adjusts the path to match the first file found (in
an order determined by the file system).

You can also use the `canonicalize()` method to convert short file names ("8.3"
names) to long file names on Windows, as in the following examples:

```haxe
import openfl.display.Sprite;
import openfl.filesystem.File;

class FileCanonicalizeExample2 extends Sprite
{
    public function new()
    {
        super();

        var path:File = new File();
        path.nativePath = "C:\\OpenFL~1";
        path.canonicalize();
        trace(path.nativePath); // C:\OpenFL Test
    }
}
```

<!-- TODO: uncomment if this is implemented in OpenFL
## Working with packages and symbolic links

Various operating systems support package files and symbolic link files:

**Packages** — On macOS, directories can be designated as packages and show up
in the macOS Finder as a single file rather than as a directory.

**Symbolic links** — macOS, Linux, and Windows Vista support symbolic links.
Symbolic links allow a file to point to another file or directory on disk.
Although similar, symbolic links are not the same as aliases. An alias is always
reported as a file (rather than a directory), and reading or writing to an alias
or shortcut never affects the original file or directory that it points to. On
the other hand, a symbolic link behaves exactly like the file or directory it
points to. It can be reported as a file or a directory, and reading or writing
to a symbolic link affects the file or directory that it points to, not the
symbolic link itself. Additionally, on Windows the `isSymbolicLink` property for
a File object referencing a junction point (used in the NTFS file system) is set
to `true`.

The File class includes the `isPackage` and `isSymbolicLink` properties for
checking if a File object references a package or symbolic link.

The following code iterates through the user's desktop directory, listing
subdirectories that are _not_ packages:

```haxe
var desktopNodes:Array<File> = File.desktopDirectory.getDirectoryListing();
for (i in 0...desktopNodes.length)
{
    if (desktopNodes[i].isDirectory && !desktopNodes[i].isPackage)
    {
        trace(desktopNodes[i].name);
    }
}
```

The following code iterates through the user's desktop directory, listing files
and directories that are _not_ symbolic links:

```haxe
var desktopNodes:Array<File> = File.desktopDirectory.getDirectoryListing();
for (i in 0...desktopNodes.length)
{
    if (!desktopNodes[i].isSymbolicLink)
    {
        trace(desktopNodes[i].name);
    }
}
```

The `canonicalize()` method changes the path of a symbolic link to point to the
file or directory to which the link refers. The following code iterates through
the user's desktop directory, and reports the paths referenced by files that are
symbolic links:

```haxe
var desktopNodes:Array<File> = File.desktopDirectory.getDirectoryListing();
for (i in 0...desktopNodes.length)
{
    if (desktopNodes[i].isSymbolicLink)
    {
        var linkNode:File = desktopNodes[i] as File;
        linkNode.canonicalize();
        trace(linkNode.nativePath);
    }
}
```
-->

<!-- TODO: uncomment if implemented in OpenFL
## Determining space available on a volume

The `spaceAvailable` property of a File object is the space available for use at
the File location, in bytes. For example, the following code checks the space
available in the application storage directory:

```haxe
trace(File.applicationStorageDirectory.spaceAvailable);
``````

If the File object references a directory, the `spaceAvailable` property
indicates the space in the directory that files can use. If the File object
references a file, the `spaceAvailable` property indicates the space into which
the file could grow. If the file location does not exist, the `spaceAvailable`
property is set to 0. If the File object references a symbolic link, the
`spaceAvailable` property is set to space available at the location the symbolic
link points to.

Typically the space available for a directory or file is the same as the space
available on the volume containing the directory or file. However, space
available can take into account quotas and per-directory limits.

Adding a file or directory to a volume generally requires more space than the
actual size of the file or the size of the contents of the directory. For
example, the operating system may require more space to store index information.
Or the disk sectors required may use additional space. Also, available space
changes dynamically. So, you cannot expect to allocate all of the reported space
for file storage. For information on writing to the file system, see
[Workflow for reading and writing files](./workflow-for-reading-and-writing-files.md).

The `StorageVolumeInfo.getStorageVolumes()` method provides more detailed
information on mounted storage volumes (see
[Working with storage volumes](./working-with-storage-volumes.md)).-->

## Opening files with the default system application

You can open a file using the application registered by the operating system to
open it. For example, an OpenFL application can open a DOC file with the
application registered to open it. Use the `openWithDefaultApplication()` method
of a File object to open the file. For example, the following code opens a file
named test.doc on the user's desktop and opens it with the default application
for DOC files:

```haxe
import openfl.display.Sprite;
import openfl.filesystem.File;

class FileOpenWithDefaultApplicationExample1 extends Sprite
{
    public function new()
    {
        super();

        var file:File = File.desktopDirectory;
        file = file.resolvePath("test.doc");
        file.openWithDefaultApplication();
    }
}
```

Note: On Linux, the file's MIME type, not the filename extension, determines the
default application for a file.

The following code lets the user navigate to an mp3 file and open it in the
default application for playing mp3 files:

```haxe
import openfl.display.Sprite;
import openfl.events.Event;
import openfl.filesystem.File;
import openfl.net.FileFilter;

class FileOpenWithDefaultApplicationExample2 extends Sprite
{
    private var file:File;

    public function new()
    {
        super();

        file = File.documentsDirectory;
        var mp3Filter:FileFilter = new FileFilter("MP3 Files", "*.mp3");
        file.addEventListener(Event.SELECT, fileSelected);
        file.browseForOpen("Open", [mp3Filter]);
    }

    private function fileSelected(e:Event):Void
    {
        file.openWithDefaultApplication();
    }
}
```

You cannot use the `openWithDefaultApplication()` method with files located in
the application directory.

Note: This limitation does not exist for an OpenFL application installed using a
native installer (an extended desktop application).
