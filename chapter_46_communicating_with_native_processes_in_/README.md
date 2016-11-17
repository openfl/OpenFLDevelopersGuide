# Chapter 46: Communicating with native processes in AIR {#chapter-46-communicating-with-native-processes-in-air}

Adobe AIR 2 and later

As of Adobe AIR 2, AIR applications can run and communicate with other native processes via the command line. For example, an AIR application can run a process and communicate with it via the standard input and output streams.

To communicate with native processes, package an AIR application to be installed via a native installer. The file type of native installer is specific to the operating system for which it is created:

*   It is a DMG file on Mac OS.
*   It is an EXE file on Windows.
*   It is an RPM or DEB package on Linux.

These applications are known as extended desktop profile applications. You can create a native installer file by specifying the -target native option when calling the -package command using ADT.

**More Help topics**

[flash.filesystem.File.openWithDefaultApplication()](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/filesystem/File.html#openWithDefaultApplication()) [flash.desktop.NativeProcess](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/desktop/NativeProcess.html)

**Overview of native process communications**

Adobe AIR 2 and later

An AIR application in the extended desktop profile can execute a file, as if it were invoked by the command line. It can communicate with the standard streams of the native process. Standard streams include the standard input stream (stdin), the output stream (stdout), the standard error stream (stderr).

**_Note:_ **_Applications in the extended desktop profile can also launch files and applications using the File.openWithDefaultApplication() method. However, using this method does not provide the AIR application with access to the standard streams. For more information, see_

_“Opening files with the default system application” on_

_page 681_

The following code sample shows how to launch a test.exe application in the application directory. The application passes the argument &quot;hello&quot; as a command-line argument, and it adds an event listener to the process’s standard output stream:

var nativeProcessStartupInfo:NativeProcessStartupInfo = new NativeProcessStartupInfo(); var file:File = File.applicationDirectory.resolvePath(&quot;test.exe&quot;); nativeProcessStartupInfo.executable = file;

var processArgs:Vector.&lt;String&gt; = new Vector.&lt;String&gt;(); processArgs.push(&quot;hello&quot;); nativeProcessStartupInfo.arguments = processArgs; process = new NativeProcess();

process.addEventListener(ProgressEvent.STANDARD_OUTPUT_DATA, onOutputData); process.start(nativeProcessStartupInfo);

public function onOutputData(event:ProgressEvent):void

{

var stdOut:ByteArray = process.standardOutput;

var data:String = stdOut.readUTFBytes(process.standardOutput.bytesAvailable); trace(&quot;Got: &quot;, data);

}