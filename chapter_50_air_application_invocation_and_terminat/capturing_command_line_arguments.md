## Capturing command line arguments {#capturing-command-line-arguments}

Adobe AIR 1.0 and later

The command line arguments associated with the invocation of an AIR application are delivered in the InvokeEvent object dispatched by the NativeApplication object. The InvokeEvent arguments property contains an array of the arguments passed by the operating system when an AIR application is invoked. If the arguments contain relative file paths, you can typically resolve the paths using the currentDirectory property.

The arguments passed to an AIR program are treated as white-space delimited strings, unless enclosed in double quotes:

| Arguments | Array |
| --- | --- |
| tick tock | {tick,tock} |
| tick &quot;tick tock&quot; | {tick,tick tock} |
| &quot;tick&quot; “tock” | {tick,tock} |
| \&quot;tick\&quot; \&quot;tock\&quot; | {&quot;tick&quot;,&quot;tock&quot;} |

The currentDirectory property of an InvokeEvent object contains a File object representing the directory from which the application was launched.

When an application is invoked because a file of a type registered by the application is opened, the native path to the file is included in the command line arguments as a string. (Your application is responsible for opening or performing the intended operation on the file.) Likewise, when an application is programmed to update itself (rather than relying on the standard AIR update user interface), the native path to the AIR file is included when a user double-clicks an AIR file containing an application with a matching application ID.

You can access the file using the resolve() method of the currentDirectory File object:

if((invokeEvent.currentDirectory != null)&amp;&amp;(invokeEvent.arguments.length &gt; 0)){ dir = invokeEvent.currentDirectory;

fileToOpen = dir.resolvePath(invokeEvent.arguments[0]);

}

You should also validate that an argument is indeed a path to a file.

**Example: Invocation event log**

Adobe AIR 1.0 and later

The following example demonstrates how to register listeners for and handle the invoke event. The example logs all the invocation events received and displays the current directory and command line arguments.

ActionScript example

package

{

import flash.display.Sprite; import flash.events.InvokeEvent;

import flash.desktop.NativeApplication; import flash.text.TextField;

public class InvokeEventLogExample extends Sprite

{

public var log:TextField;

public function InvokeEventLogExample()

{

log = new TextField(); log.x = 15;

log.y = 15;

log.width = 520;

log.height = 370; log.background = true;

addChild(log);

NativeApplication.nativeApplication.addEventListener(InvokeEvent.INVOKE, onInvoke);

}

public function onInvoke(invokeEvent:InvokeEvent):void

{

var now:String = new Date().toTimeString(); logEvent(&quot;Invoke event received: &quot; + now);

if (invokeEvent.currentDirectory != null)

{

logEvent(&quot;Current directory=&quot; + invokeEvent.currentDirectory.nativePath);

}

else

{

logEvent(&quot;--no directory information available--&quot;);

}

if (invokeEvent.arguments.length &gt; 0)

{

}

else

{

}

}

logEvent(&quot;Arguments: &quot; + invokeEvent.arguments.toString());

logEvent(&quot;--no arguments--&quot;);

public function logEvent(entry:String):void

{

log.appendText(entry + &quot;\n&quot;); trace(entry);

}

}

}

Flex example

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) layout=&quot;vertical&quot; invoke=&quot;onInvoke(event)&quot; title=&quot;Invocation Event Log&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.events.InvokeEvent;

import flash.desktop.NativeApplication;

public function onInvoke(invokeEvent:InvokeEvent):void { var now:String = new Date().toTimeString(); logEvent(&quot;Invoke event received: &quot; + now);

if (invokeEvent.currentDirectory != null){

logEvent(&quot;Current directory=&quot; + invokeEvent.currentDirectory.nativePath);

} else {

logEvent(&quot;--no directory information available--&quot;);

}

if (invokeEvent.arguments.length &gt; 0){

logEvent(&quot;Arguments: &quot; + invokeEvent.arguments.toString());

} else {

logEvent(&quot;--no arguments--&quot;);

}

}

public function logEvent(entry:String):void { log.text += entry + &quot;\n&quot;;

trace(entry);

}

]]&gt;

&lt;/mx:Script&gt;

&lt;mx:TextArea id=&quot;log&quot; width=&quot;100%&quot; height=&quot;100%&quot; editable=&quot;false&quot; valueCommit=&quot;log.verticalScrollPosition=log.textHeight;&quot;/&gt;

&lt;/mx:WindowedApplication&gt;