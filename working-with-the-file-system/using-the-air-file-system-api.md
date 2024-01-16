# Using the AIR file system API {#using-the-air-file-system-api}

Adobe AIR 1.0 and later

The Adobe AIR file system API includes the following classes:

*   File
*   FileMode
*   FileStream

The file system API lets you do the following (and more):

*   Copy, create, delete, and move files and directories
*   Get information about files and directories
*   Read and write files

**AIR file basics**

Adobe AIR 1.0 and later

For a quick explanation and code examples of working with the file system in AIR, see the following quick start articles on the Adobe Developer Connection:

*   [Building a text-file editor](http://www.adobe.com/go/learn_air_qs_textedit_flash_en) (Flash)
*   [Building a text-file editor](http://www.adobe.com/go/learn_air_qs_textedit_flex_en) (Flex)
*   [Building a directory search application](http://www.adobe.com/go/learn_air_qs_search_flex_en) (Flex)
*   [Reading and writing from an XML preferences file](http://www.adobe.com/go/learn_air_qs_xmlpref_flex_en) (Flex)
*   [Compressing files and data](http://www.adobe.com/go/learn_air_qs_compress_en) (Flex)

Adobe AIR provides classes that you can use to access, create, and manage both files and folders. These classes, contained in the openfl.filesystem package, are used as follows:

| **File classes** | **Description** |
| --- | --- |
| [File](https://api.openfl.org/openfl/filesystem/File.html) | File object represents a path to a file or directory. You use a file object to create a pointer to a file or folder, initiating interaction with the file or folder. |
| [FileMode](https://api.openfl.org/openfl/filesystem/FileMode.html) | The FileMode class defines string constants used in the fileMode parameter of the open() and openAsync() methods of the FileStream class. The fileMode parameter of these methods determines the capabilities available to the FileStream object once the file is opened, which include writing, reading, appending, and updating. |
| [FileStream](https://api.openfl.org/openfl/filesystem/FileStream.html) | FileStream object is used to open files for reading and writing. Once you’ve created a File object that points to a new or existing file, you pass that pointer to the FileStream object so that you can open it and read or write data. |

Some methods in the File class have both synchronous and asynchronous versions:

*   File.copyTo() and File.copyToAsync()
*   File.deleteDirectory() and File.deleteDirectoryAsync()
*   File.deleteFile() and File.deleteFileAsync()
*   File.getDirectoryListing() and File.getDirectoryListingAsync()
*   File.moveTo() and File.moveToAsync()
*   File.moveToTrash() and File.moveToTrashAsync()

Also, FileStream operations work synchronously or asynchronously depending on how the FileStream object opens the file: by calling the open() method or by calling the openAsync() method.

The asynchronous versions let you initiate processes that run in the background and dispatch events when complete (or when error events occur). Other code can execute while these asynchronous background processes are taking place. With asynchronous versions of the operations, you must set up event listener functions, using the addEventListener() method of the File or FileStream object that calls the function.

The synchronous versions let you write simpler code that does not rely on setting up event listeners. However, since other code cannot execute while a synchronous method is executing, important processes such as display object rendering and animation can be delayed.

## Working with File objects in AIR {#working-with-file-objects-in-air}

Adobe AIR 1.0 and later

A File object is a pointer to a file or directory in the file system.

The File class extends the FileReference class. The FileReference class, which is available in Adobe® Flash® Player as well as AIR, represents a pointer to a file. The File class adds properties and methods that are not exposed in OpenFL (in a project running in a browser), due to security considerations.

**About the File class**

Adobe AIR 1.0 and later

You can use the File class for the following:

*   Getting the path to special directories, including the user directory, the user's documents directory, the directory from which the application was launched, and the application directory
*   Coping files and directories
*   Moving files and directories
*   Deleting files and directories (or moving them to the trash)
*   Listing files and directories contained in a directory
*   Creating temporary files and folders

Once a File object points to a file path, you can use it to read and write file data, using the FileStream class.

A File object can point to the path of a file or directory that does not yet exist. You can use such a File object in creating a file or directory.

Paths of File objects

Adobe AIR 1.0 and later

Each File object has two properties that each define its path:

| **Property** | **Description** |
| --- | --- |
| nativePath | Specifies the platform-specific path to a file. For example, on Windows a path might be "c:\Sample directory\test.txt" whereas on Mac OS it could be "/Sample directory/test.txt". A nativePath property uses the backslash (\) character as the directory separator character on Windows, and it uses the forward slash (/) character on Mac OS and Linux. |
| url | This may use the file URL scheme to point to a file. For example, on Windows a path might be "file:///c:/Sample%20directory/test.txt" whereas on Mac OS it could be "file:///Sample%20directory/test.txt". The runtime includes other special URL schemes besides file and are described in

"Supported AIR URL schemes" on page 677

 |

The File class includes static properties for pointing to standard directories on Mac OS, Windows, and Linux. These properties include:

*   File.applicationStorageDirectory—a storage directory unique to each installed AIR application. This directory is an appropriate place to store dynamic application assets and user preferences. Consider storing large amounts of data elsewhere.

On Android and iOS, the application storage directory is removed when the application is uninstalled or the user chooses to clear application data, but this is not the case on other platforms.

*   File.applicationDirectory—the directory where the application is installed (along with any installed assets). On some operating systems, the application is stored in a single package file rather than a physical directory. In this case, the contents may not be accessible using the native path. The application directory is read-only.
*   File.desktopDirectory—the user’s desktop directory. If a platform does not define a desktop directory, another location on the file system is used.
*   File.documentsDirectory—the user’s documents directory. If a platform does not define a documents directory, another location on the file system is used.
*   File.userDirectory—the user directory. If a platform does not define a user directory, another location on the file system is used.

**_Note:_** _When a platform does not define standard locations for desktop, documents, or user directories,_

_File.documentsDirectory, File.desktopDirectory, and File.userDirectory can reference the same directory._

These properties have different values on different operating systems. For example, Mac and Windows each have a different native path to the user’s desktop directory. However, the File.desktopDirectory property points to an appropriate directory path on every platform. To write applications that work well across platforms, use these properties as the basis for referencing other directories and files used by the application. Then use the resolvePath() method to refine the path. For example, this code points to the preferences.xml file in the application storage directory:

var prefsFile:File = File.applicationStorageDirectory; prefsFile = prefsFile.resolvePath("preferences.xml");

Although the File class lets you point to a specific file path, doing so can lead to applications that do not work across platforms. For example, the path C:\Documents and Settings\joe\ only works on Windows. For these reasons, it is best to use the static properties of the File class, such as File.documentsDirectory.

Common directory locations

| **Platform** | **Directory type** | **Typical file system location** |
| --- | --- | --- |
| Android | Application | /data/data/ |
|  | Application-storage | /data/data/air.applicationID/filename/Local Store |
|  | Cache | /data/data/applicationID/cache |
|  | Desktop | /mnt/sdcard |
|  | Documents | /mnt/sdcard |
|  | Temporary | /data/data/applicationID/cache/FlashTmp.randomString |
|  | User | /mnt/sdcard |
| iOS | Application | /var/mobile/Applications/uid/filename.app |
|  | Application-storage | /var/mobile/Applications/uid/Library/Application Support/applicationID/Local Store |
|  | Cache | /var/mobile/Applications/uid/Library/Caches |
|  | Desktop | not accessible |
|  | Documents | /var/mobile/Applications/uid/Documents |
|  | Temporary | /private/var/mobile/Applications/uid/tmp/FlashTmpNNN |
|  | User | not accessible |
| Linux | Application | /opt/filename/share |
|  | Application-storage | /home/userName/.appdata/applicationID/Local Store |
|  | Desktop | /home/userName/Desktop |
|  | Documents | /home/userName/Documents |
|  | Temporary | /tmp/FlashTmp.randomString |
|  | User | /home/userName |
| Mac | Application | /Applications/filename.app/Contents/Resources |
|  | Application-storage | /Users/**_userName_**/Library/Preferences/**_applicationid_**/Local Store (AIR |
|  | Cache | /Users/userName/Library/Caches |
|  | Desktop /Users/**_userName_**/Desktop |
|  | Documents /Users/**_userName_**/Documents |
|  | Temporary | /private/var/folders/JY/randomString/TemporaryItems/FlashTmp |
|  | User /Users/**_userName_** |

| **Platform Directory type Typical file system location** |
| --- |
| Windows | Application | C:\Program Files\filename |
|  | Application-storage | C:\Documents and settings\userName\ApplicationData\applicationID\Local Store |
|  | Cache | C:\Documents and settings\userName\Local Settings\Temp |
|  | Desktop | C:\Documents and settings\userName\Desktop |
|  | Documents | C:\Documents and settings\userName\My Documents |
|  | Temporary | C:\Documents and settings\userName\Local Settings\Temp\randomString.tmp |
|  | User | C:\Documents and settings\userName |

The actual native paths for these directories vary based on the operating system and computer configuration. The paths shown in this table are typical examples. You should always use the appropriate static File class properties to refer to these directories so that your application works correctly on any platform. In an actual AIR application, the values for applicationID and filename shown in the table are taken from the application descriptor. If you specify a publisher ID in the application descriptor, then the publisher ID is appended to the application ID in these paths. The value for userName is the account name of the installing user.

Pointing a File object to a directory

Adobe AIR 1.0 and later

There are different ways to set a File object to point to a directory.

Pointing to the user’s home directory

**Adobe AIR 1.0 and later**

You can point a File object to the user’s home directory. The following code sets a File object to point to an AIR Test subdirectory of the home directory:

var file:File = File.userDirectory.resolvePath("AIR Test");

Pointing to the user’s documents directory

**Adobe AIR 1.0 and later**

You can point a File object to the user's documents directory. The following code sets a File object to point to an AIR Test subdirectory of the documents directory:

var file:File = File.documentsDirectory.resolvePath("AIR Test");

Pointing to the desktop directory

**Adobe AIR 1.0 and later**

You can point a File object to the desktop. The following code sets a File object to point to an AIR Test subdirectory of the desktop:

var file:File = File.desktopDirectory.resolvePath("AIR Test");

Pointing to the application storage directory

**Adobe AIR 1.0 and later**

You can point a File object to the application storage directory. For every AIR application, there is a unique associated path that defines the application storage directory. This directory is unique to each application and user. You can use this directory to store user-specific, application-specific data (such as user data or preferences files). For example, the following code points a File object to a preferences file, prefs.xml, contained in the application storage directory:

var file:File = File.applicationStorageDirectory; file = file.resolvePath("prefs.xml");

The application storage directory location is typically based on the user name and the application ID. The following file system locations are given here to help you debug your application. You should always use the File.applicationStorage property or app-storage: URI scheme to resolve files in this directory:

*   On Mac OS — varies by AIR version:

**AIR 3.2 and earlier**: /Users/user name/Library/Preferences/_applicationID_/Local Store/

**AIR 3.3 and later**: _path_/Library/Application Support/_applicationID_/Local Store, where _path_ is either

/Users/_username_/Library/Containers/_bundle-id_/Data (sandboxed environment) or /Users/_username_ ( when running outside a sandboxed environment)

For example (AIR 3.2):

/Users/babbage/Library/Preferences/com.example.TestApp/Local Store

*   On Windows—In the documents and Settings directory, in:

_C:\Documents and Settings\user name_\Application Data\_applicationID_\Local Store\

For example:

C:\Documents and Settings\babbage\Application Data\com.example.TestApp\Local Store

*   On Linux—In:

/home/_user name_/.appdata/_applicationID_/Local Store/

For example:

/home/babbage/.appdata/com.example.TestApp/Local Store

*   On Android—In:

/data/data/androidPackageID/applicationID/Local Store

For example:

/data/data/air.com.example.TestApp/com.example.TestApp/Local Store

**_Note:_** _If an application has a publisher ID, then the publisher ID is also used as part of the path to the application storage directory._

The URL (and url property) for a File object created with File.applicationStorageDirectory uses the app- storage URL scheme (see

"Supported AIR URL schemes" on page 677

), as in the following:

var dir:File = File.applicationStorageDirectory; dir = dir.resolvePath("preferences"); trace(dir.url); // app-storage:/preferences

Pointing to the application directory

**Adobe AIR 1.0 and later**

You can point a File object to the directory in which the application was installed, known as the application directory. You can reference this directory using the File.applicationDirectory property. You can use this directory to examine the application descriptor file or other resources installed with the application. For example, the following code points a File object to a directory named _images_ in the application directory:

var dir:File = File.applicationDirectory; dir = dir.resolvePath("images");

The URL (and url property) for a File object created with File.applicationDirectory uses the app URL scheme (see

"Supported AIR URL schemes" on page 677

), as in the following:

var dir:File = File.applicationDirectory; dir = dir.resolvePath("images"); trace(dir.url); // app:/images

**_Note:_** _On Android, the files in the application package are not accessible via the nativePath. The nativePath property is an empty string. Always use the URL to access files in the application directory rather than a native path._

Pointing to the cache directory

**Adobe AIR 3.6 and later**

You can point a File object to the operating system’s temporary or cache directory using the File.cacheDirectory property. This directory contains temporary files that are not required for the application to run and will not cause problems or data loss for the user if they are deleted.

In most operating systems the cache directory is a temporary directory. On iOS, the cache directory corresponds to the application library’s Caches directory. Files in this directory are not backed up to online storage, and can potentially be deleted by the operating system if the device’s available storage space is too low. For more information, see

"Controlling file backup and caching" on page 677

.

Pointing to the file system root

**Adobe AIR 1.0 and later**

The File.getRootDirectories() method lists all root volumes, such as C: and mounted volumes, on a Windows computer. On Mac OS and Linux, this method always returns the unique root directory for the machine (the "/" directory). The StorageVolumeInfo.getStorageVolumes() method provides more detailed information on mounted storage volumes (see

"Working with storage volumes" on page 687

).

**_Note:_** _The root of the file system is not readable on Android. A File object referencing the directory with the native path, "/", is returned, but the properties of that object do not have accurate values. For example, spaceAvailable is always 0._

Pointing to an explicit directory

**Adobe AIR 1.0 and later**

You can point the File object to an explicit directory by setting the nativePath property of the File object, as in the following example (on Windows):

var file:File = new File(); file.nativePath = "C:\\AIR Test";

**Important:** Pointing to an explicit path this way can lead to code that does not work across platforms. For example, the previous example only works on Windows. You can use the static properties of the File object, such as File.applicationStorageDirectory, to locate a directory that works cross-platform. Then use the resolvePath() method (see the next section) to navigate to a relative path.

Navigating to relative paths

**Adobe AIR 1.0 and later**

You can use the resolvePath() method to obtain a path relative to another given path. For example, the following code sets a File object to point to an "AIR Test" subdirectory of the user's home directory:

var file:File = File.userDirectory; file = file.resolvePath("AIR Test");

You can also use the url property of a File object to point it to a directory based on a URL string, as in the following:

var urlStr:String = "file:///C:/AIR Test/"; var file:File = new File()

file.url = urlStr;

For more information, see

"Modifying File paths" on page 676

.

Letting the user browse to select a directory

**Adobe AIR 1.0 and later**

The File class includes the browseForDirectory() method, which presents a system dialog box in which the user can select a directory to assign to the object. The browseForDirectory() method is asynchronous. The File object dispatches a select event if the user selects a directory and clicks the Open button, or it dispatches a cancel event if the user clicks the Cancel button.

For example, the following code lets the user select a directory and outputs the directory path upon selection:

var file:File = new File(); file.addEventListener(Event.SELECT, dirSelected); file.browseForDirectory("Select a directory"); function dirSelected(e:Event):void {

trace(file.nativePath);

}

**_Note:_** _On Android, the browseForDirectory() method is not supported. Calling this method has no effect; a cancel event is dispatched immediately. To allow users to select a directory, use a custom, application-defined dialog, instead._

Pointing to the directory from which the application was invoked

**Adobe AIR 1.0 and later**

You can get the directory location from which an application is invoked, by checking the currentDirectory property of the InvokeEvent object dispatched when the application is invoked. For details, see

"Capturing command line

arguments" on page 879

.

Pointing a File object to a file

Adobe AIR 1.0 and later

There are different ways to set the file to which a File object points.

Pointing to an explicit file path

**Adobe AIR 1.0 and later**

**Important:** Pointing to an explicit path can lead to code that does not work across platforms. For example, the path C:/foo.txt only works on Windows. You can use the static properties of the File object, such as File.applicationStorageDirectory, to locate a directory that works cross-platform. Then use the resolvePath() method (see

"Modifying File paths" on page 676

) to navigate to a relative path.

You can use the url property of a File object to point it to a file or directory based on a URL string, as in the following:

var urlStr:String = "file:///C:/AIR Test/test.txt"; var file:File = new File()

file.url = urlStr;

You can also pass the URL to the File() constructor function, as in the following:

var urlStr:String = "file:///C:/AIR Test/test.txt"; var file:File = new File(urlStr);

The url property always returns the URI-encoded version of the URL (for example, blank spaces are replaced with

"%20):

file.url = "file:///c:/AIR Test"; trace(file.url); // file:///c:/AIR%20Test

You can also use the nativePath property of a File object to set an explicit path. For example, the following code, when run on a Windows computer, sets a File object to the test.txt file in the AIR Test subdirectory of the C: drive:

var file:File = new File(); file.nativePath = "C:/AIR Test/test.txt";

You can also pass this path to the File() constructor function, as in the following:

var file:File = new File("C:/AIR Test/test.txt");

Use the forward slash (/) character as the path delimiter for the nativePath property. On Windows, you can also use the backslash (\) character, but doing so leads to applications that do not work across platforms.

For more information, see

"Modifying File paths" on page 676

.

Enumerating files in a directory

**Adobe AIR 1.0 and later**

You can use the getDirectoryListing() method of a File object to get an array of File objects pointing to files and subdirectories at the root level of a directory. For more information, see

"Enumerating directories" on page 683

.

Letting the user browse to select a file

**Adobe AIR 1.0 and later**

The File class includes the following methods that present a system dialog box in which the user can select a file to assign to the object:

*   browseForOpen()
*   browseForSave()
*   browseForOpenMultiple()

These methods are each asynchronous. The browseForOpen() and browseForSave() methods dispatch the select event when the user selects a file (or a target path, in the case of browseForSave()). With the browseForOpen() and browseForSave() methods, upon selection the target File object points to the selected files. The browseForOpenMultiple() method dispatches a selectMultiple event when the user selects files. The selectMultiple event is of type FileListEvent, which has a files property that is an array of File objects (pointing to the selected files).

For example, the following code presents the user with an "Open" dialog box in which the user can select a file:

var fileToOpen:File = File.documentsDirectory; selectTextFile(fileToOpen);

function selectTextFile(root:File):void

{

var txtFilter:FileFilter = new FileFilter("Text", "*.as;*.css;*.html;*.txt;*.xml"); root.browseForOpen("Open", [txtFilter]);

root.addEventListener(Event.SELECT, fileSelected);

}

function fileSelected(event:Event):void

{

trace(fileToOpen.nativePath);

}

If the application has another browser dialog box open when you call a browse method, the runtime throws an Error exception.

**_Note:_** _On Android, only image, video, and audio files can be selected with the browseForOpen() and browseForOpenMultiple() methods. The browseForSave() dialog also displays only media files even though the user can enter an arbitrary filename. For opening and saving non-media files, you should consider using custom dialogs instead of these methods._

Modifying File paths

Adobe AIR 1.0 and later

You can also modify the path of an existing File object by calling the resolvePath() method or by modifying the

nativePath or url property of the object, as in the following examples (on Windows):

var file1:File = File.documentsDirectory; file1 = file1.resolvePath("AIR Test");

trace(file1.nativePath); // C:\Documents and Settings\userName\My Documents\AIR Test var file2:File = File.documentsDirectory;

file2 = file2.resolvePath("..");

trace(file2.nativePath); // C:\Documents and Settings\userName var file3:File = File.documentsDirectory;

file3.nativePath += "/subdirectory";

trace(file3.nativePath); // C:\Documents and Settings\userName\My Documents\subdirectory var file4:File = new File();

file4.url = "file:///c:/AIR Test/test.txt"; trace(file4.nativePath); // C:\AIR Test\test.txt

When using the nativePath property, use the forward slash (/) character as the directory separator character. On Windows, you can use the backslash (\) character as well, but you should not do so, as it leads to code that does not work cross-platform.

Supported AIR URL schemes

Adobe AIR 1.0 and later

In AIR, you can use any of the following URL schemes in defining the url property of a File object:

| **URL scheme** | **Description** |
| --- | --- |
| file | Use to specify a path relative to the root of the file system. For example: |
| app | Use to specify a path relative to the root directory of the installed application (the directory that contains the application.xml file for the installed application). For example, the following path points to an images subdirectory of the directory of the installed application: |
| app-storage | Use to specify a path relative to the application store directory. For each installed application, AIR defines a unique application store directory, which is a useful place to store data specific to that application. For example, the following path points to a prefs.xml file in a settings subdirectory of the application store directory: |

Controlling file backup and caching

Adobe AIR 3.6 and later, iOS and OS X only

Certain operating systems, most notably iOS and Mac OS X, provide users the ability to automatically back up application files to a remote storage. In addition, on iOS there are restrictions on whether files can be backed up and also where files of different purposes can be stored.

The following summarize how to comply with Apple’s guidelines for file backup and storage. For further information see the next sections.

*   To specify that a file does not need to be backed up and (iOS only) can be deleted by the operating system if device storage space runs low, save the file in the cache directory (File.cacheDirectory). This is the preferred storage location on iOS and should be used for most files that can be regenerated or re-downloaded.
*   To specify that a file does not need to be backed up, but should not be deleted by the operating system, save the file in one of the application library directories such as the application storage directory (File.applicationStorageDirectory) or the documents directory (File.documentsDirectory). Set the File object’s preventBackup property to true. This is required by Apple for content that can be regenerated or downloaded again, but which is required for proper functioning of your application during offline use.

Specifying files for backup

In order to save backup space and reduce network bandwidth use, Apple’s guidelines for iOS and Mac applications specify that only files that contain user-entered data or data that otherwise can’t be regenerated or re-downloaded should be designated for backup.

By default all files in the application library folders are backed up. On Mac OS X this is the application storage directory. On iOS, this includes the application storage directory, the application directory, the desktop directory, documents directory, and user directory (because those directories are mapped to application library folders on iOS). Consequently, any files in those directories are backed up to server storage by default.

If you are saving a file in one of those locations that can be re-created by your application, you should flag the file so the operating system knows not to back it up. To indicate that a file should not be backed up, set the File object’s preventBackup property to true.

Note that on iOS, for a file in any of the application library folders, even if the file’s preventBackup property is set to

true the file is flagged as a persistent file that the operating system shouldn’t delete.

Controlling file caching and deletion

Apple’s guidelines for iOS applications specify that as much as possible, content that can be regenerated should be made available to the operating system to delete in case the device runs low on storage space.

On iOS, files in the application library folders (such as the application storage directory or the documents directory) are flagged as permanent and are not deleted by the operating system.

Save files that can be regenerated by the application and are safe to delete in case of low storage space in the application cache directory. You access the cache directory using the File.cacheDirectory static property.

On iOS the cache directory corresponds to the application’s cache directory (&lt;Application Home&gt;/Library/Caches). On other operating systems, this directory is mapped to a comparable directory. For example, on Mac OS X it also maps to the Caches directory in the application library. On Android the cache directory maps to the application’s cache directory. On Windows, the cache directory maps to the operating system temp directory. On both Android and Windows, this is the same directory that is accessed by a call to the File class’s createTempDirectory() and createTempFile() methods.

Finding the relative path between two files

Adobe AIR 1.0 and later

You can use the getRelativePath() method to find the relative path between two files:

var file1:File = File.documentsDirectory.resolvePath("AIR Test"); var file2:File = File.documentsDirectory

file2 = file2.resolvePath("AIR Test/bob/test.txt"); trace(file1.getRelativePath(file2)); // bob/test.txt

The second parameter of the getRelativePath() method, the useDotDot parameter, allows for .. syntax to be returned in results, to indicate parent directories:

var file1:File = File.documentsDirectory; file1 = file1.resolvePath("AIR Test"); var file2:File = File.documentsDirectory;

file2 = file2.resolvePath("AIR Test/bob/test.txt"); var file3:File = File.documentsDirectory;

file3 = file3.resolvePath("AIR Test/susan/test.txt");

trace(file2.getRelativePath(file1, true)); // ../.. trace(file3.getRelativePath(file2, true)); // ../../bob/test.txt

Obtaining canonical versions of file names

Adobe AIR 1.0 and later

File and path names are not case sensitive on Windows and Mac OS. In the following, two File objects point to the same file:

File.documentsDirectory.resolvePath("test.txt"); File.documentsDirectory.resolvePath("TeSt.TxT");

However, documents and directory names do include capitalization. For example, the following assumes that there is a folder named AIR Test in the documents directory, as in the following examples:

var file:File = File.documentsDirectory.resolvePath("AIR test"); trace(file.nativePath); // ... AIR test

file.canonicalize(); trace(file.nativePath); // ... AIR Test

The canonicalize() method converts the nativePath object to use the correct capitalization for the file or directory name. On case sensitive file systems (such as Linux), when multiple files exists with names differing only in case, the canonicalize() method adjusts the path to match the first file found (in an order determined by the file system).

You can also use the canonicalize() method to convert short file names ("8.3" names) to long file names on Windows, as in the following examples:

var path:File = new File(); path.nativePath = "C:\\AIR~1"; path.canonicalize(); trace(path.nativePath); // C:\AIR Test

Working with packages and symbolic links

Adobe AIR 1.0 and later

Various operating systems support package files and symbolic link files:

**Packages**—On Mac OS, directories can be designated as packages and show up in the Mac OS Finder as a single file rather than as a directory.

**Symbolic links**—Mac OS, Linux, and Windows Vista support symbolic links. Symbolic links allow a file to point to another file or directory on disk. Although similar, symbolic links are not the same as aliases. An alias is always reported as a file (rather than a directory), and reading or writing to an alias or shortcut never affects the original file or directory that it points to. On the other hand, a symbolic link behaves exactly like the file or directory it points to. It can be reported as a file or a directory, and reading or writing to a symbolic link affects the file or directory that it points to, not the symbolic link itself. Additionally, on Windows the isSymbolicLink property for a File object referencing a junction point (used in the NTFS file system) is set to true.

The File class includes the isPackage and isSymbolicLink properties for checking if a File object references a package or symbolic link.

The following code iterates through the user’s desktop directory, listing subdirectories that are _not_ packages:

var desktopNodes:Array = File.desktopDirectory.getDirectoryListing(); for (var i:uint = 0; i &lt; desktopNodes.length; i++)

{

if (desktopNodes[i].isDirectory &amp;&amp; !desktopNodes[i].isPackage)

{

trace(desktopNodes[i].name);

}

}

The following code iterates through the user’s desktop directory, listing files and directories that are _not_ symbolic links:

var desktopNodes:Array = File.desktopDirectory.getDirectoryListing(); for (var i:uint = 0; i &lt; desktopNodes.length; i++)

{

if (!desktopNodes[i].isSymbolicLink)

{

trace(desktopNodes[i].name);

}

}

The canonicalize() method changes the path of a symbolic link to point to the file or directory to which the link refers. The following code iterates through the user’s desktop directory, and reports the paths referenced by files that are symbolic links:

var desktopNodes:Array = File.desktopDirectory.getDirectoryListing(); for (var i:uint = 0; i &lt; desktopNodes.length; i++)

{

if (desktopNodes[i].isSymbolicLink)

{

var linkNode:File = desktopNodes[i] as File; linkNode.canonicalize(); trace(linkNode.nativePath);

}

}

Determining space available on a volume

Adobe AIR 1.0 and later

The spaceAvailable property of a File object is the space available for use at the File location, in bytes. For example, the following code checks the space available in the application storage directory:

trace(File.applicationStorageDirectory.spaceAvailable);

If the File object references a directory, the spaceAvailable property indicates the space in the directory that files can use. If the File object references a file, the spaceAvailable property indicates the space into which the file could grow. If the file location does not exist, the spaceAvailable property is set to 0\. If the File object references a symbolic link, the spaceAvailable property is set to space available at the location the symbolic link points to.

Typically the space available for a directory or file is the same as the space available on the volume containing the directory or file. However, space available can take into account quotas and per-directory limits.

Adding a file or directory to a volume generally requires more space than the actual size of the file or the size of the contents of the directory. For example, the operating system may require more space to store index information. Or the disk sectors required may use additional space. Also, available space changes dynamically. So, you cannot expect to allocate all of the reported space for file storage. For information on writing to the file system, see

"Reading and

writing files" on page 689

.

The StorageVolumeInfo.getStorageVolumes() method provides more detailed information on mounted storage volumes (see

"Working with storage volumes" on page 687

).

Opening files with the default system application

Adobe AIR 2 and later

In AIR 2, you can open a file using the application registered by the operating system to open it. For example, an AIR application can open a DOC file with the application registered to open it. Use the openWithDefaultApplication() method of a File object to open the file. For example, the following code opens a file named test.doc on the user’s desktop and opens it with the default application for DOC files:

var file:File = File.deskopDirectory; file = file.resolvePath("test.doc"); file.openWithDefaultApplication();

**_Note:_** _On Linux, the file’s MIME type, not the filename extension, determines the default application for a file._

The following code lets the user navigate to an mp3 file and open it in the default application for playing mp3 files:

var file:File = File.documentsDirectory;

var mp3Filter:FileFilter = new FileFilter("MP3 Files", "*.mp3"); file.browseForOpen("Open", [mp3Filter]); file.addEventListener(Event.SELECT, fileSelected);

function fileSelected(e:Event):void

{

file.openWithDefaultApplication();

}

You cannot use the openWithDefaultApplication() method with files located in the application directory.

AIR prevents you from using the openWithDefaultApplication() method to open certain files. On Windows, AIR prevents you from opening files that have certain filetypes, such as EXE or BAT. On Mac OS and Linux, AIR prevents you from opening files that will launch in certain application. (These include Terminal and AppletLauncher on Mac OS; and csh, bash, or ruby on Linux.) Attempting to open one of these files using the openWithDefaultApplication() method results in an exception. For a complete list of prevented filetypes, see the language reference entry for the File.openWithDefaultApplication() method.

**_Note:_** _This limitation does not exist for an AIR application installed using a native installer (an extended desktop application)._

## Getting file system information {#getting-file-system-information}

Adobe AIR 1.0 and later

The File class includes the following static properties that provide some useful information about the file system:

| **Property** | **Description** |
| --- | --- |
| File.lineEnding | The line-ending character sequence used by the host operating system. On Mac OS and Linux, this is the line-feed character. On Windows, this is the carriage return character followed by the line-feed character. |
| File.separator | The host operating system's path component separator character. On Mac OS and Linux, this is the forward slash (/) character. On Windows, it is the backslash (\) character. |
| File.systemCharset | The default encoding used for files by the host operating system. This pertains to the character set used by the operating system, corresponding to its language. |

The Capabilities class also includes useful system information that can be useful when working with files:

| **Property** | **Description** |
| --- | --- |
| Capabilities.hasIME | Specifies whether the player is running on a system that does (true) or does not (false) have an input method editor (IME) installed. |
| Capabilities.language | Specifies the language code of the system on which the player is running. |
| Capabilities.os | Specifies the current operating system. |

**_Note:_** _Be careful when using Capabilities.os to determine system characteristics. If a more specific property exists to determine a system characteristic, use it. Otherwise, you run the risk of writing code that does not work correctly on all platforms. For example, consider the following code:_

var separator:String;

if (Capablities.os.indexOf("Mac") &gt; -1)

{

separator = "/";

}

else

{

separator = "\\";

}

This code leads to problems on Linux. It is better to simply use the File.separator property.

## Working with directories {#working-with-directories}

Adobe AIR 1.0 and later

The runtime provides you with capabilities to work with directories on the local file system.

For details on creating File objects that point to directories, see

"Pointing a File object to a directory" on page 671

.

**Creating directories**

Adobe AIR 1.0 and later

The File.createDirectory() method lets you create a directory. For example, the following code creates a directory named AIR Test as a subdirectory of the user's home directory:

var dir:File = File.userDirectory.resolvePath("AIR Test"); dir.createDirectory();

If the directory exists, the createDirectory() method does nothing.

Also, in some modes, a FileStream object creates directories when opening files. Missing directories are created when you instantiate a FileStream instance with the fileMode parameter of the FileStream() constructor set to FileMode.APPEND or FileMode.WRITE. For more information, see

"Workflow for reading and writing files" on

page 689

.

Creating a temporary directory

Adobe AIR 1.0 and later

The File class includes a createTempDirectory() method, which creates a directory in the temporary directory folder for the System, as in the following example:

var temp:File = File.createTempDirectory();

The createTempDirectory() method automatically creates a unique temporary directory (saving you the work of determining a new unique location).

You can use a temporary directory to temporarily store temporary files used for a session of the application. Note that there is a createTempFile() method for creating new, unique temporary files in the System temporary directory.

You may want to delete the temporary directory before closing the application, as it is _not_ automatically deleted on all devices.

Enumerating directories

Adobe AIR 1.0 and later

You can use the getDirectoryListing() method or the getDirectoryListingAsync() method of a File object to get an array of File objects pointing to files and subfolders in a directory.

For example, the following code lists the contents of the user's documents directory (without examining subdirectories):

var directory:File = File.documentsDirectory;

var contents:Array = directory.getDirectoryListing(); for (var i:uint = 0; i &lt; contents.length; i++)

{

trace(contents[i].name, contents[i].size);

}

When using the asynchronous version of the method, the directoryListing event object has a files property that is the array of File objects pertaining to the directories:

var directory:File = File.documentsDirectory; directory.getDirectoryListingAsync(); directory.addEventListener(FileListEvent.DIRECTORY_LISTING, dirListHandler);

function dirListHandler(event:FileListEvent):void

{

var contents:Array = event.files;

for (var i:uint = 0; i &lt; contents.length; i++)

{

trace(contents[i].name, contents[i].size);

}

}

Copying and moving directories

Adobe AIR 1.0 and later

You can copy or move a directory, using the same methods as you would to copy or move a file. For example, the following code copies a directory synchronously:

var sourceDir:File = File.documentsDirectory.resolvePath("AIR Test");

var resultDir:File = File.documentsDirectory.resolvePath("AIR Test Copy"); sourceDir.copyTo(resultDir);

When you specify true for the overwrite parameter of the copyTo() method, all files and folders in an existing target directory are deleted and replaced with the files and folders in the source directory (even if the target file does not exist in the source directory).

The directory that you specify as the newLocation parameter of the copyTo() method specifies the path to the resulting directory; it does _not_ specify the _parent_ directory that will contain the resulting directory.

For details, see

"Copying and moving files" on page 685

.

Deleting directory contents

Adobe AIR 1.0 and later

The File class includes a deleteDirectory() method and a deleteDirectoryAsync() method. These methods delete directories, the first working synchronously, the second working asynchronously (see

"AIR file basics" on

page 667

). Both methods include a deleteDirectoryContents parameter (which takes a Boolean value); when this parameter is set to true (the default value is false) the call to the method deletes non-empty directories; otherwise, only empty directories are deleted.

For example, the following code synchronously deletes the AIR Test subdirectory of the user's documents directory:

var directory:File = File.documentsDirectory.resolvePath("AIR Test"); directory.deleteDirectory(true);

The following code asynchronously deletes the AIR Test subdirectory of the user's documents directory:

var directory:File = File.documentsDirectory.resolvePath("AIR Test"); directory.addEventListener(Event.COMPLETE, completeHandler) directory.deleteDirectoryAsync(true);

function completeHandler(event:Event):void {
	trace("Deleted.")

}

Also included are the moveToTrash() and moveToTrashAsync() methods, which you can use to move a directory to the System trash. For details, see

"Moving a file to the trash" on page 687

.

## Working with files {#working-with-files}

Adobe AIR 1.0 and later

Using the AIR file API, you can add basic file interaction capabilities to your applications. For example, you can read and write files, copy and delete files, and so on. Since your applications can access the local file system, refer to

"AIR

security" on page 1076

, if you haven't already done so.

**_Note:_** _You can associate a file type with an AIR application (so that double-clicking it opens the application). For details, see_

_"Managing file associations" on page 888_

_._

**Getting file information**

Adobe AIR 1.0 and later

The File class includes the following properties that provide information about a file or directory to which a File object points:

| **File property** | **Description** |
| --- | --- |
| creationDate | The creation date of the file on the local disk. |
| creator | Obsolete—use the extension property. (This property reports the Macintosh creator type of the file, which is only used in Mac OS versions prior to Mac OS X.) |
| downloaded | (AIR 2 and later) Indicates whether the referenced file or directory was downloaded (from the internet) or not. property is only meaningful on operating systems in which files can be flagged as downloaded: |
| exists | Whether the referenced file or directory exists. |
| extension | The file extension, which is the part of the name following (and not including) the final dot ("."). If there is no dot in the filename, the extension is null. |
| icon | An Icon object containing the icons defined for the file. |
| isDirectory | Whether the File object reference is to a directory. |
| modificationDate | The date that the file or directory on the local disk was last modified. |
| name | The name of the file or directory (including the file extension, if there is one) on the local disk. |
| nativePath | The full path in the host operating system representation. See

"Paths of File objects" on page 668

. |
| parent | The folder that contains the folder or file represented by the File object. This property is null if the File object references a file or directory in the root of the file system. |
| size | The size of the file on the local disk in bytes. |
| type | Obsolete—use the extension property. (On the Macintosh, this property is the four-character file type, which is only used in Mac OS versions prior to Mac OS X.) |
| url | The URL for the file or directory. See

"Paths of File objects" on page 668

. |

For details on these properties, see the File class entry in the [OpenFL API Reference](https://api.openfl.org/openfl/filesystem/File.html).

Copying and moving files

Adobe AIR 1.0 and later

The File class includes two methods for copying files or directories: copyTo() and copyToAsync(). The File class includes two methods for moving files or directories: moveTo() and moveToAsync(). The copyTo() and moveTo() methods work synchronously, and the copyToAsync() and moveToAsync() methods work asynchronously (see

"AIR

file basics" on page 667

).

To copy or move a file, you set up two File objects. One points to the file to copy or move, and it is the object that calls the copy or move method; the other points to the destination (result) path.

The following copies a test.txt file from the AIR Test subdirectory of the user's documents directory to a file named copy.txt in the same directory:

var original:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var newFile:File = File.resolvePath("AIR Test/copy.txt"); original.copyTo(newFile, true);

In this example, the value of overwrite parameter of the copyTo() method (the second parameter) is set to true. By setting overwrite to true, an existing target file is overwritten. This parameter is optional. If you set it to false (the default value), the operation dispatches an IOErrorEvent event if the target file exists (and the file is not copied).

The "Async" versions of the copy and move methods work asynchronously. Use the addEventListener() method to monitor completion of the task or error conditions, as in the following code:

var original = File.documentsDirectory;

original = original.resolvePath("AIR Test/test.txt");

var destination:File = File.documentsDirectory;

destination = destination.resolvePath("AIR Test 2/copy.txt");

original.addEventListener(Event.COMPLETE, fileMoveCompleteHandler); original.addEventListener(IOErrorEvent.IO_ERROR, fileMoveIOErrorEventHandler); original.moveToAsync(destination);

function fileMoveCompleteHandler(event:Event):void {
	trace(event.target); // [object File]

}

function fileMoveIOErrorEventHandler(event:IOErrorEvent):void {
	trace("I/O Error.");

}

The File class also includes the File.moveToTrash() and File.moveToTrashAsync() methods, which move a file or directory to the system trash.

Deleting a file

Adobe AIR 1.0 and later

The File class includes a deleteFile() method and a deleteFileAsync() method. These methods delete files, the first working synchronously, the second working asynchronously (see

"AIR file basics" on page 667

).

For example, the following code synchronously deletes the test.txt file in the user's documents directory:

var file:File = File.documentsDirectory.resolvePath("test.txt"); file.deleteFile();

The following code asynchronously deletes the test.txt file of the user's documents directory:

var file:File = File.documentsDirectory.resolvePath("test.txt"); file.addEventListener(Event.COMPLETE, completeHandler) file.deleteFileAsync();

function completeHandler(event:Event):void {
	trace("Deleted.")

}

Also included are the moveToTrash() and moveToTrashAsync methods, which you can use to move a file or directory to the System trash. For details, see

"Moving a file to the trash" on page 687

.

Moving a file to the trash

Adobe AIR 1.0 and later

The File class includes a moveToTrash() method and a moveToTrashAsync() method. These methods send a file or directory to the System trash, the first working synchronously, the second working asynchronously (see

"AIR file

basics" on page 667

).

For example, the following code synchronously moves the test.txt file in the user's documents directory to the System trash:

var file:File = File.documentsDirectory.resolvePath("test.txt"); file.moveToTrash();

**_Note:_** _On operating systems that do not support the concept of a recoverable trash folder, the files are removed immediately._

Creating a temporary file

Adobe AIR 1.0 and later

The File class includes a createTempFile() method, which creates a file in the temporary directory folder for the System, as in the following example:

var temp:File = File.createTempFile();

The createTempFile() method automatically creates a unique temporary file (saving you the work of determining a new unique location).

You can use a temporary file to temporarily store information used in a session of the application. Note that there is also a createTempDirectory() method, for creating a unique temporary directory in the System temporary directory.

You may want to delete the temporary file before closing the application, as it is _not_ automatically deleted on all devices.

## Working with storage volumes {#working-with-storage-volumes}

Adobe AIR 2 and later

In AIR 2, you can detect when mass storage volumes are mounted or unmounted. The StorageVolumeInfo class defines a singleton storageVolumeInfo object. The StorageVolumeInfo.storageVolumeInfo object dispatches a storageVolumeMount event when a storage volume is mounted. And it dispatches a storageVolumeUnmount event when a volume is unmounted. The StorageVolumeChangeEvent class defines these events.

**_Note:_** _On modern Linux distributions, the StorageVolumeInfo object only dispatches storageVolumeMount and_

_storageVolumeUnmount events for physical devices and network drives mounted at particular locations._

The storageVolume property of the StorageVolumeChangeEvent class is a StorageVolume object. The StorageVolume class defines basic properties of the storage volume:

*   drive—The volume drive letter on Windows (null on other operating systems)
*   fileSystemType—The type of file system on the storage volume (such as "FAT", "NTFS", "HFS", or "UFS")
*   isRemoveable—Whether a volume is removable (true) or not (false)
*   isWritable—Whether a volume is writable (true) or not (false)
*   name—The name of the volume
*   rootDirectory—A File object corresponding to the root directory of the volume

The StorageVolumeChangeEvent class also includes a rootDirectory property. The rootDirectory property is a File object referencing the root directory of the storage volume that has been mounted or unmounted.

The storageVolume property of the StorageVolumeChangeEvent object is undefined (null) for an unmounted volume. However you can access the rootDirectory property of the event.

The following code outputs the name and file path of a storage volume when it is mounted:

StorageVolumeInfo.storageVolumeInfo.addEventListener(StorageVolumeChangeEvent.STORAGE_VOLUME

_MOUNT, onVolumeMount);

function onVolumeMount(event:StorageVolumeChangeEvent):void

{

trace(event.storageVolume.name, event.rootDirectory.nativePath);

}

The following code outputs the file path of a storage volume when it is unmounted:

StorageVolumeInfo.storageVolumeInfo.addEventListener(StorageVolumeChangeEvent.STORAGE_VOLUME

_UNMOUNT, onVolumeUnmount);

function onVolumeUnmount(event:StorageVolumeChangeEvent):void

{

trace(event.rootDirectory.nativePath);

}

The StorageVolumeInfo.storageVolumeInfo object includes a getStorageVolumes() method. This method returns a vector of StorageVolume objects corresponding to the currently mounted storage volumes. The following code shows how to list the names and root directories of all mounted storage volumes:

var volumes:Vector.&lt;StorageVolume&gt; = new Vector.&lt;StorageVolume&gt;; volumes = StorageVolumeInfo.storageVolumeInfo.getStorageVolumes(); for (var i:int = 0; i &lt; volumes.length; i++)

{

trace(volumes[i].name, volumes[i].rootDirectory.nativePath);

}

**_Note:_** _On modern Linux distributions, the getStorageVolumes() method returns objects corresponding to physical devices and network drives mounted at particular locations._

The File.getRootDirectories() method lists the root directories (see ["Pointing to the file system root" on](#696154181379824-_bookmark409) [page 673](#696154181379824-_bookmark409). However, the StorageVolume objects (enumerated by the StorageVolumeInfo.getStorageVolumes() method) provides more information about the storage volumes.

You can use the spaceAvailable property of the rootDirectory property of a StorageVolume object to get the space available on a storage volume. (See

"Determining space available on a volume" on page 680

.)

**More Help topics**

[StorageVolume](https://api.openfl.org/openfl/filesystem/StorageVolume.html) [StorageVolumeInfo](https://api.openfl.org/openfl/filesystem/StorageVolumeInfo.html)

## Reading and writing files {#reading-and-writing-files}

Adobe AIR 1.0 and later

The [FileStream](https://api.openfl.org/openfl/filesystem/FileStream.html) class lets AIR applications read and write to the file system.

**Workflow for reading and writing files**

Adobe AIR 1.0 and later

The workflow for reading and writing files is as follows.

Initialize a File object that points to the path.

The File object represents the path of the file that you want to work with (or a file that you will later create).

var file:File = File.documentsDirectory;

file = file.resolvePath("AIR Test/testFile.txt");

This example uses the File.documentsDirectory property and the resolvePath() method of a File object to initialize the File object. However, there are many other ways to point a File object to a file. For more information, see

"Pointing a File object to a file" on page 674

.

Initialize a FileStream object.

**Call the open() method or the openAsync() method of the FileStream object.**

The method you call depends on whether you want to open the file for synchronous or asynchronous operations. Use the File object as the file parameter of the open method. For the fileMode parameter, specify a constant from the FileMode class that specifies the way in which you will use the file.

For example, the following code initializes a FileStream object that is used to create a file and overwrite any existing data:

var fileStream:FileStream = new FileStream(); fileStream.open(file, FileMode.WRITE);

For more information, see ["Initializing a FileStream object, and opening and closing files" on page 691](#696154181379824-_bookmark423) and ["FileStream open modes" on page 690](#696154181379824-_bookmark422).

If you opened the file asynchronously (using the openAsync() method), add and set up event listeners for the FileStream object.

These event listener methods respond to events dispatched by the FileStream object in various situations. These situations include when data is read in from the file, when I/O errors are encountered, or when the complete amount of data to be written has been written.

For details, see ["Asynchronous programming and the events generated by a FileStream object opened asynchronously"](#696154181379824-_bookmark425) [on page 695](#696154181379824-_bookmark425).

Include code for reading and writing data, as needed.

There are many methods of the FileStream class related to reading and writing. (They each begin with "read" or "write".) The method you choose to use to read or write data depends on the format of the data in the target file.

For example, if the data in the target file is UTF-encoded text, you may use the readUTFBytes() and writeUTFBytes() methods. If you want to deal with the data as byte arrays, you may use the readByte(), readBytes(), writeByte(), and writeBytes() methods. For details, see ["Data formats, and choosing the read and](#696154181379824-_bookmark426) [write methods to use" on page 695](#696154181379824-_bookmark426).

If you opened the file asynchronously, then be sure that enough data is available before calling a read method. For details, see ["The read buffer and the bytesAvailable property of a FileStream object" on page 693](#696154181379824-_bookmark424).

Before writing to a file, if you want to check the amount of disk space available, you can check the spaceAvailable property of the File object. For more information, see

"Determining space available on a volume" on page 680

.

Call the close() method of the FileStream object when you are done working with the file.

Calling the close() method makes the file available to other applications.

For details, see ["Initializing a FileStream object, and opening and closing files" on page 691](#696154181379824-_bookmark423).

To see a sample application that uses the FileStream class to read and write files, see the following articles at the Adobe AIR Developer Center:

*   [Building a text-file editor](http://www.adobe.com/go/learn_air_qs_textedit_flash_en)
*   [Building a text-file editor](http://www.adobe.com/go/learn_air_qs_textedit_flex_en)
*   [Reading and writing from an XML preferences file](http://www.adobe.com/go/learn_air_qs_xmlpref_flex_en)

Working with FileStream objects

Adobe AIR 1.0 and later

The FileStream class defines methods for opening, reading, and writing files.

FileStream open modes

**Adobe AIR 1.0 and later**

The open() and openAsync() methods of a FileStream object each include a fileMode parameter, which defines some properties for a file stream, including the following:

*   The ability to read from the file
*   The ability to write to the file
*   Whether data will always be appended past the end of the file (when writing)
*   What to do when the file does not exist (and when its parent directories do not exist)

The following are the various file modes (which you can specify as the fileMode parameter of the open() and

openAsync() methods):

| **File mode** | **Description** |
| --- | --- |
| FileMode.READ | Specifies that the file is open for reading only. |

| **File mode** | **Description** |
| --- | --- |
| FileMode.WRITE | Specifies that the file is open for writing. If the file does not exist, it is created when the FileStream object is opened. If the file does exist, any existing data is deleted. |
| FileMode.APPEND | Specifies that the file is open for appending. The file is created if it does not exist. If the file exists, existing data is not overwritten, and all writing begins at the end of the file. |
| FileMode.UPDATE | Specifies that the file is open for reading and writing. If the file does not exist, it is created. Specify this mode for random read/write access to the file. You can read from any position in the file. When writing to the file, only the bytes written overwrite existing bytes (all other bytes remain unchanged). |

Initializing a FileStream object, and opening and closing files

**Adobe AIR 1.0 and later**

When you open a FileStream object, you make it available to read and write data to a file. You open a FileStream object by passing a File object to the open() or openAsync() method of the FileStream object:

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream();

myFileStream.open(myFile, FileMode.READ);

The fileMode parameter (the second parameter of the open() and openAsync() methods), specifies the mode in which to open the file: for read, write, append, or update. For details, see the previous section, ["FileStream open](#696154181379824-_bookmark422) [modes" on page 690](#696154181379824-_bookmark422).

If you use the openAsync() method to open the file for asynchronous file operations, set up event listeners to handle the asynchronous events:

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream(); myFileStream.addEventListener(Event.COMPLETE, completeHandler); myFileStream.addEventListener(ProgressEvent.PROGRESS, progressHandler); myFileStream.addEventListener(IOErrorEvent.IO_Error, errorHandler); myFileStream.openAsync(myFile, FileMode.READ);

function completeHandler(event:Event):void {

// ...

}

function progressHandler(event:ProgressEvent):void {

// ...

}

function errorHandler(event:IOErrorEvent):void {

// ...

}

The file is opened for synchronous or asynchronous operations, depending upon whether you use the open() or

openAsync() method. For details, see

"AIR file basics" on page 667

.

If you set the fileMode parameter to FileMode.READ or FileMode.UPDATE in the open method of the FileStream object, data is read into the read buffer as soon as you open the FileStream object. For details, see ["The read buffer and](#696154181379824-_bookmark424) [the bytesAvailable property of a FileStream object" on page 693](#696154181379824-_bookmark424).

You can call the close() method of a FileStream object to close the associated file, making it available for use by other applications.

The position property of a FileStream object

**Adobe AIR 1.0 and later**

The position property of a FileStream object determines where data is read or written on the next read or write method.

Before a read or write operation, set the position property to any valid position in the file.

For example, the following code writes the string "hello" (in UTF encoding) at position 8 in the file:

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream();

myFileStream.open(myFile, FileMode.UPDATE); myFileStream.position = 8; myFileStream.writeUTFBytes("hello");

When you first open a FileStream object, the position property is set to 0.

Before a read operation, the value of position must be at least 0 and less than the number of bytes in the file (which are existing positions in the file).

The value of the position property is modified only in the following conditions:

*   When you explicitly set the position property.
*   When you call a read method.
*   When you call a write method.

When you call a read or write method of a FileStream object, the position property is immediately incremented by the number of bytes that you read or write. Depending on the read method you use, the position property is either incremented by the number of bytes you specify to read or by the number of bytes available. When you call a read or write method subsequently, it reads or writes starting at the new position.

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream();

myFileStream.open(myFile, FileMode.UPDATE); myFileStream.position = 4000;

trace(myFileStream.position); // 4000

myFileStream.writeBytes(myByteArray, 0, 200);

trace(myFileStream.position); // 4200

There is, however, one exception: for a FileStream opened in append mode, the position property is not changed after a call to a write method. (In append mode, data is always written to the end of the file, independent of the value of the position property.)

For a file opened for asynchronous operations, the write operation does not complete before the next line of code is executed. However, you can call multiple asynchronous methods sequentially, and the runtime executes them in order:

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream(); myFileStream.openAsync(myFile, FileMode.WRITE); myFileStream.writeUTFBytes("hello");

myFileStream.writeUTFBytes("world"); myFileStream.addEventListener(Event.CLOSE, closeHandler); myFileStream.close();

trace("started.");

closeHandler(event:Event):void

{

trace("finished.");

}

The trace output for this code is the following:

started. finished.

You _can_ specify the position value immediately after you call a read or write method (or at any time), and the next read or write operation will take place starting at that position. For example, note that the following code sets the position property right after a call to the writeBytes() operation, and the position is set to that value (300) even after the write operation completes:

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream(); myFileStream.openAsync(myFile, FileMode.UPDATE);

myFileStream.position = 4000;

trace(myFileStream.position); // 4000

myFileStream.writeBytes(myByteArray, 0, 200);

myFileStream.position = 300;

trace(myFileStream.position); // 300

The read buffer and the bytesAvailable property of a FileStream object

**Adobe AIR 1.0 and later**

When a FileStream object with read capabilities (one in which the fileMode parameter of the open() or openAsync() method was set to READ or UPDATE) is opened, the runtime stores the data in an internal buffer. The FileStream object begins reading data into the buffer as soon as you open the file (by calling the open() or openAsync() method of the FileStream object).

For a file opened for synchronous operations (using the open() method), you can always set the position pointer to any valid position (within the bounds of the file) and begin reading any amount of data (within the bounds of the file), as shown in the following code (which assumes that the file contains at least 100 bytes):

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream();

myFileStream.open(myFile, FileMode.READ); myFileStream.position = 10;

myFileStream.readBytes(myByteArray, 0, 20);

myFileStream.position = 89;

myFileStream.readBytes(myByteArray, 0, 10);

Whether a file is opened for synchronous or asynchronous operations, the read methods always read from the "available" bytes, represented by the bytesAvalable property. When reading synchronously, all of the bytes of the file are available all of the time. When reading asynchronously, the bytes become available starting at the position specified by the position property, in a series of asynchronous buffer fills signaled by progress events.

For files opened for _synchronous_ operations, the bytesAvailable property is always set to represent the number of bytes from the position property to the end of the file (all bytes in the file are always available for reading).

For files opened for _asynchronous_ operations, you need to ensure that the read buffer has consumed enough data before calling a read method. For a file opened asynchronously, as the read operation progresses, the data from the file, starting at the position specified when the read operation started, is added to the buffer, and the bytesAvailable property increments with each byte read. The bytesAvailable property indicates the number of bytes available starting with the byte at the position specified by the position property to the end of the buffer. Periodically, the FileStream object sends a progress event.

For a file opened asynchronously, as data becomes available in the read buffer, the FileStream object periodically dispatches the progress event. For example, the following code reads data into a ByteArray object, bytes, as it is read into the buffer:

var bytes:ByteArray = new ByteArray();

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream(); myFileStream.addEventListener(ProgressEvent.PROGRESS, progressHandler); myFileStream.openAsync(myFile, FileMode.READ);

function progressHandler(event:ProgressEvent):void

{

myFileStream.readBytes(bytes, myFileStream.position, myFileStream.bytesAvailable);

}

For a file opened asynchronously, only the data in the read buffer can be read. Furthermore, as you read the data, it is removed from the read buffer. For read operations, you need to ensure that the data exists in the read buffer before calling the read operation. For example, the following code reads 8000 bytes of data starting from position 4000 in the file:

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream(); myFileStream.addEventListener(ProgressEvent.PROGRESS, progressHandler); myFileStream.addEventListener(Event.COMPLETE, completed); myFileStream.openAsync(myFile, FileMode.READ);

myFileStream.position = 4000; var str:String = "";

function progressHandler(event:Event):void

{

if (myFileStream.bytesAvailable &gt; 8000 )

{

str += myFileStream.readMultiByte(8000, "iso-8859-1");

}

}

During a write operation, the FileStream object does not read data into the read buffer. When a write operation completes (all data in the write buffer is written to the file), the FileStream object starts a new read buffer (assuming that the associated FileStream object was opened with read capabilities), and starts reading data into the read buffer, starting from the position specified by the position property. The position property may be the position of the last byte written, or it may be a different position, if the user specifies a different value for the position object after the write operation.

Asynchronous programming and the events generated by a FileStream object opened asynchronously

**Adobe AIR 1.0 and later**

When a file is opened asynchronously (using the openAsync() method), reading and writing files are done asynchronously. As data is read into the read buffer and as output data is being written, other Haxe code can execute.

This means that you need to register for events generated by the FileStream object opened asynchronously.

By registering for the progress event, you can be notified as new data becomes available for reading, as in the following code:

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream(); myFileStream.addEventListener(ProgressEvent.PROGRESS, progressHandler); myFileStream.openAsync(myFile, FileMode.READ);

var str:String = "";

function progressHandler(event:ProgressEvent):void

{

str += myFileStream.readMultiByte(myFileStream.bytesAvailable, "iso-8859-1");

}

You can read the entire data by registering for the complete event, as in the following code:

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream(); myFileStream.addEventListener(Event.COMPLETE, completed); myFileStream.openAsync(myFile, FileMode.READ);

var str:String = "";

function completeHandler(event:Event):void

{

str = myFileStream.readMultiByte(myFileStream.bytesAvailable, "iso-8859-1");

}

In much the same way that input data is buffered to enable asynchronous reading, data that you write on an asynchronous stream is buffered and written to the file asynchronously. As data is written to a file, the FileStream object periodically dispatches an OutputProgressEvent object. An OutputProgressEvent object includes a bytesPending property that is set to the number of bytes remaining to be written. You can register for the outputProgress event to be notified as this buffer is actually written to the file, perhaps in order to display a progress dialog. However, in general, it is not necessary to do so. In particular, you may call the close() method without concern for the unwritten bytes. The FileStream object will continue writing data and the close event will be delivered after the final byte is written to the file and the underlying file is closed.

Data formats, and choosing the read and write methods to use

**Adobe AIR 1.0 and later**

Every file is a set of bytes on a disk. In Haxe, the data from a file can always be represented as a ByteArray. For example, the following code reads the data from a file into a ByteArray object named bytes:

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream(); myFileStream.addEventListener(Event.COMPLETE, completeHandler); myFileStream.openAsync(myFile, FileMode.READ);

var bytes:ByteArray = new ByteArray();

function completeHandler(event:Event):void

{

myFileStream.readBytes(bytes, 0, myFileStream.bytesAvailable);

}

Similarly, the following code writes data from a ByteArray named bytes to a file:

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream();

myFileStream.open(myFile, FileMode.WRITE); myFileStream.writeBytes(bytes, 0, bytes.length);

However, often you do not want to store the data in an Haxe ByteArray object. And often the data file is in a specified file format.

For example, the data in the file may be in a text file format, and you may want to represent such data in a String object.

For this reason, the FileStream class includes read and write methods for reading and writing data to and from types other than ByteArray objects. For example, the readMultiByte() method lets you read data from a file and store it to a string, as in the following code:

var myFile:File = File.documentsDirectory.resolvePath("AIR Test/test.txt"); var myFileStream:FileStream = new FileStream(); myFileStream.addEventListener(Event.COMPLETE, completed); myFileStream.openAsync(myFile, FileMode.READ);

var str:String = "";

function completeHandler(event:Event):void

{

str = myFileStream.readMultiByte(myFileStream.bytesAvailable, "iso-8859-1");

}

The second parameter of the readMultiByte() method specifies the text format that Haxe uses to interpret the data ("iso-8859-1" in the example). Adobe AIR supports common character set encodings (see Supported character sets).

The FileStream class also includes the readUTFBytes() method, which reads data from the read buffer into a string using the UTF-8 character set. Since characters in the UTF-8 character set are of variable length, do not use readUTFBytes() in a method that responds to the progress event, since the data at the end of the read buffer may represent an incomplete character. (This is also true when using the readMultiByte() method with a variable-length character encoding.) For this reason, read the entire set of data when the FileStream object dispatches the complete event.

There are also similar write methods, writeMultiByte() and writeUTFBytes(), for working with String objects and text files.

The readUTF() and the writeUTF() methods (not to be confused with readUTFBytes() and writeUTFBytes()) also read and write the text data to a file, but they assume that the text data is preceded by data specifying the length of the text data, which is not a common practice in standard text files.

Some UTF-encoded text files begin with a "UTF-BOM" (byte order mark) character that defines the endianness as well as the encoding format (such as UTF-16 or UTF-32).

For an example of reading and writing to a text file, see

"Example: Reading an XML file into an XML object" on

page 698

.

The readObject() and writeObject() are convenient ways to store and retrieve data for complex Haxe objects. The data is encoded in AMF (Haxe Message Format). Adobe AIR, OpenFL, Flash Media Server, and Flex Data Services include APIs for working with data in this format.

There are some other read and write methods (such as readDouble() and writeDouble()). However, if you use these, make sure that the file format matches the formats of the data defined by these methods.

File formats are often more complex than simple text formats. For example, an MP3 file includes compressed data that can only be interpreted with the decompression and decoding algorithms specific to MP3 files. MP3 files also may include ID3 tags that contain meta tag information about the file (such as the title and artist for a song). There are multiple versions of the ID3 format, but the simplest (ID3 version 1) is discussed in the

"Example: Reading and writing

data with random access" on page 699

section.

Other files formats (for images, databases, application documents, and so on) have different structures, and to work with their data in Haxe, you must understand how the data is structured.

Using the load() and save() methods

OpenFL 10 and later, Adobe AIR 1.5 and later

OpenFL 10 added the load() and save() methods to the FileReference class. These methods are also in AIR 1.5, and the File class inherits the methods from the FileReference class. These methods were designed to provide a secure means for users to load and save file data in OpenFL. However, AIR applications can also use these methods as an easy way to load and save files asynchronously.

For example, the following code saves a string to a text file:

var file:File = File.applicationStorageDirectory.resolvePath("test.txt"); var str:String = "Hello.";

file.addEventListener(Event.COMPLETE, fileSaved); file.save(str);

function fileSaved(event:Event):void

{

trace("Done.");

}

The data parameter of the save() method can take a String, XML, or ByteArray value. When the argument is a String or XML value, the method saves the file as a UTF-8–encoded text file.

When this code sample executes, the application displays a dialog box in which the user selects the saved file destination.

The following code loads a string from a UTF-8–encoded text file:

var file:File = File.applicationStorageDirectory.resolvePath("test.txt"); file.addEventListener(Event.COMPLETE, loaded);

file.load(); var str:String;

function loaded(event:Event):void

{

var bytes:ByteArray = file.data;

str = bytes.readUTFBytes(bytes.length); trace(str);

}

The FileStream class provides more functionality than that provided by the load() and save() methods:

*   Using the FileStream class, you can read and write data both synchronously and asynchronously.
*   Using the FileStream class lets you write incrementally to a file.
*   Using the FileStream class lets you open a file for random access (both reading from and writing to any section of the file).
*   The FileStream class lets you specify the type of file access you have to the file, by setting the fileMode parameter of the open() or openAsync() method.
*   The FileStream class lets you save data to files without presenting the user with an Open or Save dialog box.
*   You can directly use types other than byte arrays when reading data with the FileStream class.

Example: Reading an XML file into an XML object

Adobe AIR 1.0 and later

The following examples demonstrate how to read and write to a text file that contains XML data.

To read from the file, initialize the File and FileStream objects, call the readUTFBytes() method of the FileStream and convert the string to an XML object:

var file:File = File.documentsDirectory.resolvePath("AIR Test/preferences.xml"); var fileStream:FileStream = new FileStream();

fileStream.open(file, FileMode.READ);

var prefsXML:XML = XML(fileStream.readUTFBytes(fileStream.bytesAvailable)); fileStream.close();

Similarly, writing the data to the file is as easy as setting up appropriate File and FileStream objects, and then calling a write method of the FileStream object. Pass the string version of the XML data to the write method as in the following code:

var prefsXML:XML = &lt;prefs&gt;&lt;autoSave&gt;true&lt;/autoSave&gt;&lt;/prefs&gt;;

var file:File = File.documentsDirectory.resolvePath("AIR Test/preferences.xml"); fileStream = new FileStream();

fileStream.open(file, FileMode.WRITE);

var outputString:String = '&lt;?xml version="1.0" encoding="utf-8"?&gt;\n'; outputString += prefsXML.toXMLString();

fileStream.writeUTFBytes(outputString); fileStream.close();

These examples use the readUTFBytes() and writeUTFBytes() methods, because they assume that the files are in UTF-8 format. If not, you may need to use a different method (see ["Data formats, and choosing the read and write](#696154181379824-_bookmark426) [methods to use" on page 695](#696154181379824-_bookmark426)).

The previous examples use FileStream objects opened for synchronous operation. You can also open files for asynchronous operations (which rely on event listener functions to respond to events). For example, the following code shows how to read an XML file asynchronously:

var file:File = File.documentsDirectory.resolvePath("AIR Test/preferences.xml"); var fileStream:FileStream = new FileStream(); fileStream.addEventListener(Event.COMPLETE, processXMLData); fileStream.openAsync(file, FileMode.READ);

var prefsXML:XML;

function processXMLData(event:Event):void

{

prefsXML = XML(fileStream.readUTFBytes(fileStream.bytesAvailable)); fileStream.close();

}

The processXMLData() method is invoked when the entire file is read into the read buffer (when the FileStream object dispatches the complete event). It calls the readUTFBytes() method to get a string version of the read data, and it creates an XML object, prefsXML, based on that string.

To see a sample application that shows these capabilities, see [Reading and writing from an XML preferences file](http://www.adobe.com/go/learn_air_qs_xmlpref_flex_en).

Example: Reading and writing data with random access

Adobe AIR 1.0 and later

MP3 files can include ID3 tags, which are sections at the beginning or end of the file that contain meta data identifying the recording. The ID3 tag format itself has different revisions. This example describes how to read and write from an MP3 file that contains the simplest ID3 format (ID3 version 1.0) using "random access to file data", which means that it reads from and writes to arbitrary locations in the file.

An MP3 file that contains an ID3 version 1 tag includes the ID3 data at the end of the file, in the final 128 bytes. When accessing a file for random read/write access, it is important to specify FileMode.UPDATE as the fileMode

parameter for the open() or openAsync() method:

var file:File = File.documentsDirectory.resolvePath("My Music/Sample ID3 v1.mp3"); var fileStr:FileStream = new FileStream();

fileStr.open(file, FileMode.UPDATE);

This lets you both read and write to the file.

Upon opening the file, you can set the position pointer to the position 128 bytes before the end of the file:

fileStr.position = file.size - 128;

This code sets the position property to this location in the file because the ID3 v1.0 format specifies that the ID3 tag data is stored in the last 128 bytes of the file. The specification also says the following:

*   The first 3 bytes of the tag contain the string "TAG".
*   The next 30 characters contain the title for the MP3 track, as a string.
*   The next 30 characters contain the name of the artist, as a string.
*   The next 30 characters contain the name of the album, as a string.
*   The next 4 characters contain the year, as a string.
*   The next 30 characters contain the comment, as a string.
*   The next byte contains a code indicating the track's genre.
*   All text data is in ISO 8859-1 format.

The id3TagRead() method checks the data after it is read in (upon the complete event):

function id3TagRead():void

{

if (fileStr.readMultiByte(3, "iso-8859-1").match(/tag/i))

{

var id3Title:String = fileStr.readMultiByte(30, "iso-8859-1"); var id3Artist:String = fileStr.readMultiByte(30, "iso-8859-1"); var id3Album:String = fileStr.readMultiByte(30, "iso-8859-1"); var id3Year:String = fileStr.readMultiByte(4, "iso-8859-1");

var id3Comment:String = fileStr.readMultiByte(30, "iso-8859-1"); var id3GenreCode:String = fileStr.readByte().toString(10);

}

}

You can also perform a random-access write to the file. For example, you could parse the id3Title variable to ensure that it is correctly capitalized (using methods of the String class), and then write a modified string, called newTitle, to the file, as in the following:

fileStr.position = file.length - 125; // 128 - 3 fileStr.writeMultiByte(newTitle, "iso-8859-1");

To conform with the ID3 version 1 standard, the length of the newTitle string should be 30 characters, padded at the end with the character code 0 (String.fromCharCode(0)).