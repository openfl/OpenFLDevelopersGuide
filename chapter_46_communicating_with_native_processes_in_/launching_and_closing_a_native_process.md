## Launching and closing a native process {#launching-and-closing-a-native-process}

Adobe AIR 2 and later

To launch a native process, set up a NativeProcessInfo object to do the following:

*   Point to the file you want to launch
*   Store command-line arguments to pass to the process when launched (optional)
*   Set the working directory of the process (optional)

To start the native process, pass the NativeProcessInfo object as the parameter of the start() method of a NativeProcess object.

For example, the following code shows how to launch a test.exe application in the application directory. The application passes the argument &quot;hello&quot; and sets the user’s documents directory as the working directory:

var nativeProcessStartupInfo:NativeProcessStartupInfo = new NativeProcessStartupInfo(); var file:File = File.applicationDirectory.resolvePath(&quot;test.exe&quot;); nativeProcessStartupInfo.executable = file;

var processArgs:Vector.&lt;String&gt; = new Vector.&lt;String&gt;(); processArgs[0] = &quot;hello&quot;; nativeProcessStartupInfo.arguments = processArgs;

nativeProcessStartupInfo.workingDirectory = File.documentsDirectory; process = new NativeProcess(); process.start(nativeProcessStartupInfo);

To terminate the process, call the exit() method of the NativeProcess object.

If you want a file to be executable in your installed application, make sure that it&#039;s executable on the file system when you package your application. (On Mac and Linux, you can use chmod to set the executable flag, if needed.)