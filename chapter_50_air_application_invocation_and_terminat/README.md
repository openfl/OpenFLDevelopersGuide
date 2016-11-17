# Chapter 50: AIR application invocation and termination {#chapter-50-air-application-invocation-and-termination}

Adobe AIR 1.0 and later

This section discusses the ways in which an installed Adobe® AIR® application can be invoked, as well as options and considerations for closing a running application.

**_Note:_ **_The NativeApplication, InvokeEvent, and BrowserInvokeEvent objects are only available to SWF content running in the AIR application sandbox. SWF content running in the Flash Player runtime, within the browser or the standalone player (projector), or in an AIR application outside the application sandbox, cannot access these classes._

For a quick explanation and code examples of invoking and terminating AIR applications, see the following quick start articles on the Adobe Developer Connection:

• [Startup Options](http://www.adobe.com/go/learn_air_qs_startup_options_flex_en)

**More Help topics**

[flash.desktop.NativeApplication](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/desktop/NativeApplication.html) [flash.events.InvokeEvent](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/events/InvokeEvent.html) [flash.events.BrowserInvokeEvent](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/events/BrowserInvokeEvent.html)

**Application invocation**

Adobe AIR 1.0 and later

An AIR application is invoked when the user (or the operating system):

• Launches the application from the desktop shell.

• Uses the application as a command on a command line shell.

• Opens a type of file for which the application is the default opening application.

• (Mac OS X) clicks the application icon in the dock taskbar (whether or not the application is currently running).

• Chooses to launch the application from the installer (either at the end of a new installation process, or after double- clicking the AIR file for an already installed application).

• Begins an update of an AIR application when the installed version has signaled that it is handling application updates itself (by including a &lt;customUpdateUI&gt;true&lt;/customUpdateUI&gt; declaration in the application descriptor file).

• (iOS) Receives a notification from the Apple Push Notification service (APNs).

• Invokes the application via a URL.

• Visits a web page hosting a Flash badge or application that calls com.adobe.air.AIR launchApplication() method specifying the identifying information for the AIR application. (The application descriptor must also include a &lt;allowBrowserInvocation&gt;true&lt;/allowBrowserInvocation&gt; declaration for browser invocation to succeed.)

Whenever an AIR application is invoked, AIR dispatches an InvokeEvent object of type invoke through the singleton NativeApplication object. To allow an application time to initialize itself and register an event listener, invoke events are queued instead of discarded. As soon as a listener is registered, all the queued events are delivered.

**_Note:_ **_When an application is invoked using the browser invocation feature, the NativeApplication object only dispatches an invoke event if the application is not already running._

To receive invoke events, call the addEventListener() method of the NativeApplication object (NativeApplication.nativeApplication). When an event listener registers for an invoke event, it also receives all invoke events that occurred before the registration. Queued invoke events are dispatched one at a time on a short interval after the call to addEventListener() returns. If a new invoke event occurs during this process, it may be dispatched before one or more of the queued events. This event queuing allows you to handle any invoke events that have occurred before your initialization code executes. Keep in mind that if you add an event listener later in execution (after application initialization), it will still receive all invoke events that have occurred since the application started.

Only one instance of an AIR application is started. When an already running application is invoked again, AIR dispatches a new invoke event to the running instance. It is the responsibility of an AIR application to respond to an invoke event and take the appropriate action (such as opening a new document window).

An InvokeEvent object contains any arguments passed to the application, as well as the directory from which the application has been invoked. If the application was invoked because of a file-type association, then the full path to the file is included in the command line arguments. Likewise, if the application was invoked because of an application update, the full path to the update AIR file is provided.

When multiple files are opened in one operation a single InvokeEvent object is dispatched on Mac OS X. Each file is included in the arguments array. On Windows and Linux, a separate InvokeEvent object is dispatched for each file.

Your application can handle invoke events by registering a listener with its NativeApplication object: NativeApplication.nativeApplication.addEventListener(InvokeEvent.INVOKE, onInvokeEvent); And defining an event listener:

var arguments:Array; var currentDir:File;

public function onInvokeEvent(invocation:InvokeEvent):void { arguments = invocation.arguments;

currentDir = invocation.currentDirectory;

}