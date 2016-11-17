## Invoking an AIR application on user login {#invoking-an-air-application-on-user-login}

Adobe AIR 1.0 and later

An AIR application can be set to launch automatically when the current user logs in by setting the NativeApplication startAtLogin property to true. Once set, the application automatically starts whenever the user logs in. It continues to launch at login until the setting is changed to false, the user manually changes the setting through the operating system, or the application is uninstalled. Launching at login is a run-time setting. The setting only applies to the current user. The application must be installed to successfully set the startAtLogin property to true. An error is thrown if the property is set when an application is not installed (such as when it is launched with ADL).

**_Note:_ **_The application does not launch when the computer system starts. It launches when the user logs in._

To determine whether an application has launched automatically or as a result of a user action, you can examine the reason property of the InvokeEvent object. If the property is equal to InvokeEventReason.LOGIN, then the application started automatically. For other invocation paths, the reason property is set as follows:

• InvokeEventReason.NOTIFICATION (iOS only) - The application was invoked through APNs. For more information on APNs, see Use push notifications.

• InvokeEventReason.OPEN_URL - The application was invoked by another application or by the system.

• InvokeEventReason.Standard - All other cases.

To access the reason property, your application must target AIR 1.5.1 or higher (by setting the correct namespace value in the application descriptor file).

The following, simplified application uses the InvokeEvent reason property to decide how to behave when an invoke event occurs. If the reason property is &quot;login&quot;, then the application remains in the background. Otherwise, it makes the main application visible. An application using this pattern typically starts at login so that it can carry out background processing or event monitoring and opens a window in response to a user-triggered invoke event.

package {

import flash.desktop.InvokeEventReason; import flash.desktop.NativeApplication; import flash.display.Sprite;

import flash.events.InvokeEvent;

public class StartAtLogin extends Sprite

{

public function StartAtLogin()

{

try

{

}

NativeApplication.nativeApplication.startAtLogin = true;

catch ( e:Error )

{

trace( &quot;Cannot set startAtLogin:&quot; + e.message );

}

NativeApplication.nativeApplication.addEventListener( InvokeEvent.INVOKE, onInvoke );

}

private function onInvoke( event:InvokeEvent ):void

{

if( event.reason == InvokeEventReason.LOGIN )

{

}

else

{

}

}

}

}

//do background processing...

trace( &quot;Running in background...&quot; );

this.stage.nativeWindow.activate();

**_Note:_ **_To see the difference in behavior, package and install the application. The startAtLogin property can only be set for installed applications._