## Application termination {#application-termination}

Adobe AIR 1.0 and later

The quickest way to terminate an application is to call the NativeApplication exit() method. This works fine when your application has no data to save or external resources to clean up. Calling exit() closes all windows and then terminates the application. However, to allow windows or other components of your application to interrupt the termination process, perhaps to save vital data, dispatch the proper warning events before calling exit().

Another consideration in gracefully shutting down an application is providing a single execution path, no matter how the shut-down process starts. The user (or operating system) can trigger application termination in the following ways:

• By closing the last application window when NativeApplication.nativeApplication.autoExit is true.

• By selecting the application exit command from the operating system; for example, when the user chooses the exit application command from the default menu. (This only happens on Mac OS; Windows and Linux do not provide an application exit command through system chrome.)

• By shutting down the computer.

When an exit command is mediated through the operating system by one of these routes, the NativeApplication dispatches an exiting event. If no listeners cancel the exiting event, any open windows are closed. Each window dispatches a closing and then a close event. If any of the windows cancel the closing event, the shut-down process stops.

If the order of window closure is an issue for your application, listen for the exiting event from the NativeApplication and close the windows in the proper order yourself. You might need to do this, for example, if you have a document window with tool palettes. It could be inconvenient, or worse, if the system closed the palettes, but the user decided to cancel the exit command to save some data. On Windows, the only time you will get the exiting event is after closing the last window (when the autoExit property of the NativeApplication object is set to true).

To provide consistent behavior on all platforms, whether the exit sequence is initiated via operating system chrome, menu commands, or application logic, observe the following good practices for exiting the application:

1.  Always dispatch an exiting event through the NativeApplication object before calling exit() in application code and check that another component of your application doesn’t cancel the event.

public function applicationExit():void {

var exitingEvent:Event = new Event(Event.EXITING, false, true); NativeApplication.nativeApplication.dispatchEvent(exitingEvent); if (!exitingEvent.isDefaultPrevented()) {

NativeApplication.nativeApplication.exit();

}

}

1.  Listen for the application exiting event from the NativeApplication.nativeApplication object and, in the handler, close any windows (dispatching a closing event first). Perform any needed clean-up tasks, such as saving application data or deleting temporary files, after all windows have been closed. Only use synchronous methods during cleanup to ensure that they finish before the application quits.

If the order in which your windows are closed doesn’t matter, then you can loop through the NativeApplication.nativeApplication.openedWindows array and close each window in turn. If order _does_ matter, provide a means of closing the windows in the correct sequence.

private function onExiting(exitingEvent:Event):void { var winClosingEvent:Event;

for each (var win:NativeWindow in NativeApplication.nativeApplication.openedWindows) { winClosingEvent = new Event(Event.CLOSING,false,true); win.dispatchEvent(winClosingEvent);

if (!winClosingEvent.isDefaultPrevented()) { win.close();

} else {

exitingEvent.preventDefault();

}

}

if (!exitingEvent.isDefaultPrevented()) {

//perform cleanup

}

}

1.  Windows should always handle their own clean up by listening for their own closing events.
2.  Only use one exiting listener in your application since handlers called earlier cannot know whether subsequent handlers will cancel the exiting event (and it would be unwise to rely on the order of execution).