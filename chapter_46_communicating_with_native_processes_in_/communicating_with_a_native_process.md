## Communicating with a native process {#communicating-with-a-native-process}

Adobe AIR 2 and later

Once an AIR application has started a native process, it can communicate with the standard input, standard output, and standard error streams of the process.

You read and write data to the streams using the following properties of the NativeProcess object:

*   standardInput—Contains access to the standard input stream data.
*   standardOutput—Contains access to the standard output stream data.
*   standardError—Contains access to the standard error stream data.

Writing to the standard input stream

You can write data to the standard input stream using the write methods of the standardInput property of the NativeProcess object. As the AIR application writes data to the process, the NativeProcess object dispatches standardInputProgress events.

If an error occurs in writing to the standard input stream, the NativeProcess object dispatches an

ioErrorStandardInput event.

You can close the input stream by calling the closeInput() method of the NativeProcess object. When the input stream closes, the NativeProcess object dispatches a standardInputClose event.

var nativeProcessStartupInfo:NativeProcessStartupInfo = new NativeProcessStartupInfo(); var file:File = File.applicationDirectory.resolvePath(&quot;test.exe&quot;); nativeProcessStartupInfo.executable = file;

process = new NativeProcess(); process.start(nativeProcessStartupInfo); process.standardInput.writeUTF(&quot;foo&quot;); if(process.running)

{

process.closeInput();

}

Reading from the standard output stream

You can read data from the standard output stream using the read methods of this property. As the AIR application gets output stream data from the process, the NativeProcess object dispatches standardOutputData events.

If an error occurs in writing to the standard output stream, the NativeProcess object dispatches a

standardOutputError event.

When process closes the output stream, the NativeProcess object dispatches a standardOutputClose event.

When reading data from the standard input stream, be sure to read data as it is generated. In other words, add an event listener for the standardOutputData event. In the standardOutputData event listener, read the data from the standardOutput property of the NativeProcess object. Do not simply wait for the standardOutputClose event or the exit event to read all data. If you do not read data as the native process generates the data, the buffer can fill up, or data can be lost. A full buffer can cause the native process to stall when trying to write more data. However, if you do not register an event listener for the standardOutputData event, then the buffer will not fill and the process will not stall. In this case, you will not have access to the data.

var nativeProcessStartupInfo:NativeProcessStartupInfo = new NativeProcessStartupInfo(); var file:File = File.applicationDirectory.resolvePath(&quot;test.exe&quot;); nativeProcessStartupInfo.executable = file;

process = new NativeProcess(); process.addEventListener(ProgressEvent.STANDARD_OUTPUT_DATA, dataHandler); process.start(nativeProcessStartupInfo);

var bytes:ByteArray = new ByteArray(); function dataHandler(event:ProgressEvent):void

{

bytes.writeBytes(process.standardOutput.readBytes(process.standardOutput.bytesAvailable);

}

Reading from the standard error stream

You can read data from the standard error stream using the read methods of this property. As the AIR application reads error stream data from the process, the NativeProcess object dispatches standardErrorData events.

If an error occurs in writing to the standard error stream, the NativeProcess object dispatches an

standardErrorIoError event.

When process closes the error stream, the NativeProcess object dispatches a standardErrorClose event.

When reading data from the standard error stream, be sure to read data as it is generated. In other words, add an event listener for the standardErrorData event. In the standardErrorData event listener, read the data from the standardError property of the NativeProcess object. Do not simply wait for the standardErrorClose event or the exit event to read all data. If you do not read data as the native process generates the data, the buffer can fill up, or data can be lost. A full buffer can cause the native process to stall when trying to write more data. However, if you do not register an event listener for the standardErrorData event, then the buffer will not fill and the process will not stall. In this case, you will not have access to the data.

var nativeProcessStartupInfo:NativeProcessStartupInfo = new NativeProcessStartupInfo(); var file:File = File.applicationDirectory.resolvePath(&quot;test.exe&quot;); nativeProcessStartupInfo.executable = file;

process = new NativeProcess(); process.addEventListener(ProgressEvent.STANDARD_ERROR_DATA, errorDataHandler); process.start(nativeProcessStartupInfo);

var errorBytes:ByteArray = new ByteArray(); function errorDataHandler(event:ProgressEvent):void

{

bytes.writeBytes(process.standardError.readBytes(process.standardError.bytesAvailable);

}