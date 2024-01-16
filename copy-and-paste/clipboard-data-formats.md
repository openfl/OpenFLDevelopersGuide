# Clipboard data formats {#clipboard-data-formats}

OpenFL 10 and later, Adobe AIR 1.0 and later

Clipboard formats describe the data placed in a Clipboard object. OpenFL automatically translates the standard data formats between Haxe data types and system clipboard formats. In addition, application objects can be transferred within and between Haxe-based applications using application-defined formats.

A Clipboard object can contain representations of the same information in different formats. For example, a Clipboard object representing a Sprite could include a reference format for use within the same application, a serialized format for use by another application running in OpenFL, a bitmap format for use by an image editor, and a file list format, perhaps with deferred rendering to encode a PNG file, for copying or dragging a representation of the Sprite to the file system.

**Standard data formats**

OpenFL 10 and later, Adobe AIR 1.0 and later

The constants defining the standard format names are provided in the ClipboardFormats class:

| **Constant** | **Description** |
| --- | --- |
| TEXT_FORMAT | Text-format data is translated to and from the Haxe String class. |
| HTML_FORMAT | Text with HTML markup. |
| RICH_TEXT_FORMAT | Rich-text-format data is translated to and from the Haxe ByteArray class. The RTF markup is not interpreted or translated in any way. |
| BITMAP_FORMAT | (AIR only) Bitmap-format data is translated to and from the Haxe BitmapData class. |
| FILE_LIST_FORMAT | (AIR only) File-list-format data is translated to and from an array of Haxe File objects. |
| URL_FORMAT | (AIR only) URL-format data is translated to and from the Haxe String class. |

When copying and pasting data in response to a copy, cut, or paste event in HTML content hosted in an AIR application, MIME types must be used instead of the ClipboardFormat strings. The valid data MIME types are:

| **MIME type** | **Description** |
| --- | --- |
| Text | &quot;text/plain&quot; |
| URL | &quot;text/uri-list&quot; |
| Bitmap | &quot;image/x-vnd.adobe.air.bitmap&quot; |
| File list | &quot;application/x-vnd.adobe.air.file-list&quot; |

**_Note:_** _Rich text format data is not available from the clipboardData property of the event object dispatched as a result of a paste event within HTML content._

## Custom data formats {#custom-data-formats}

OpenFL 10 and later, Adobe AIR 1.0 and later

You can use application-defined custom formats to transfer objects as references or as serialized copies. References are valid only within the same application. Serialized objects can be transferred between applications, but can be used only with objects that remain valid when serialized and deserialized. Objects can usually be serialized if their properties are either simple types or serializable objects.

To add a serialized object to a Clipboard object, set the _serializable_ parameter to true when calling the Clipboard.setData() method. The format name can be one of the standard formats or an arbitrary string defined by your application.

**Transfer modes**

OpenFL 10 and later, Adobe AIR 1.0 and later

When an object is written to the clipboard using a custom data format, the object data can be read from the clipboard either as a reference or as a serialized copy of the original object. There are four transfer modes that determine whether objects are transferred as references or as serialized copies:

| **Transfer mode** | **Description** |
| --- | --- |
| ClipboardTransferModes.ORIGINAL_ONLY | Only a reference is returned. If no reference is available, a null value is returned. |
| ClipboardTransferModes.ORIGINAL_PREFFERED | A reference is returned, if available. Otherwise a serialized copy is returned. |
| ClipboardTransferModes.CLONE_ONLY | Only a serialized copy is returned. If no serialized copy is available, a null value is returned. |
| ClipboardTransferModes.CLONE_PREFFERED | A serialized copy is returned, if available. Otherwise a reference is returned. |

Reading and writing custom data formats

OpenFL 10 and later, Adobe AIR 1.0 and later

When writing an object to the clipboard, you can use any string that does not begin with the reserved prefixes air: or flash: for the _format_ parameter. Use the same string as the format to read the object. The following examples illustrate how to read and write objects to the clipboard:

public function createClipboardObject(object:Object):Clipboard{ var transfer:Clipboard = Clipboard.generalClipboard; transfer.setData(&quot;object&quot;, object, true);

}

To extract a serialized object from the clipboard object (after a drop or paste operation), use the same format name and the CLONE_ONLY or CLONE_PREFFERED transfer modes.

var transfer:Object = clipboard.getData(&quot;object&quot;, ClipboardTransferMode.CLONE_ONLY);

A reference is always added to the Clipboard object. To extract the reference from the clipboard object (after a drop or paste operation), instead of the serialized copy, use the ORIGINAL_ONLY or ORIGINAL_PREFFERED transfer modes:

var transferredObject:Object =

clipboard.getData(&quot;object&quot;, ClipboardTransferMode.ORIGINAL_ONLY);

References are valid only if the Clipboard object originates from the current application. Use the ORIGINAL_PREFFERED transfer mode to access the reference when it is available, and the serialized clone when the reference is not available.

## Deferred rendering {#deferred-rendering}

OpenFL 10 and later, Adobe AIR 1.0 and later

If creating a data format is computationally expensive, you can use deferred rendering by supplying a function that supplies the data on demand. The function is called only if a receiver of the drop or paste operation requests data in the deferred format.

The rendering function is added to a Clipboard object using the setDataHandler() method. The function must return the data in the appropriate format. For example, if you called setDataHandler(ClipboardFormat.TEXT_FORMAT, writeText), then the writeText() function must return a string.

If a data format of the same type is added to a Clipboard object with the setData() method, that data takes precedence over the deferred version (the rendering function is never called). The rendering function may or may not be called again if the same clipboard data is accessed a second time.

**_Note:_** _On Mac OS X, deferred rendering works only with custom data formats. With standard data formats, the rendering function is called immediately._

**Pasting text using a deferred rendering function**

OpenFL 10 and later, Adobe AIR 1.0 and later

The following example illustrates how to implement a deferred rendering function.

When the user presses the Copy button, the application clears the system clipboard to ensure that no data is left over from previous clipboard operations. The setDataHandler() method then sets the renderData() function as the clipboard renderer.

When the user selects the Paste command from the context menu of the destination text field, the application accesses the clipboard and sets the destination text. Since the text data format on the clipboard has been set with a function rather than a string, the clipboard calls the renderData() function. The renderData() function returns the text in the source text, which is then assigned to the destination text.

Notice that if you edit the source text before pressing the Paste button, the edit will be reflected in the pasted text, even when the edit occurs after the copy button was pressed. This is because the rendering function doesn’t copy the source text until the paste button is pressed. (When using deferred rendering in a real application, you might want to store or protect the source data in some way to prevent this problem.)

Flash example

package {

import openfl.desktop.Clipboard;

import openfl.desktop.ClipboardFormats; import openfl.desktop.ClipboardTransferMode; import openfl.display.Sprite;

import openfl.text.TextField; import openfl.text.TextFormat; import openfl.text.TextFieldType; import openfl.events.MouseEvent; import openfl.events.Event;

public class DeferredRenderingExample extends Sprite

{

private var sourceTextField:TextField; private var destination:TextField; private var copyText:TextField;

public function DeferredRenderingExample():void

{

sourceTextField = createTextField(10, 10, 380, 90); sourceTextField.text = &quot;Neque porro quisquam est qui dolorem &quot;

+ &quot;ipsum quia dolor sit amet, consectetur, adipisci velit.&quot;;

copyText = createTextField(10, 110, 35, 20); copyText.htmlText = &quot;&lt;a href=&#039;#&#039;&gt;Copy&lt;/a&gt;&quot;; copyText.addEventListener(MouseEvent.CLICK, onCopy);

destination = createTextField(10, 145, 380, 90); destination.addEventListener(Event.PASTE, onPaste);

}

private function createTextField(x:Number, y:Number, width:Number, height:Number):TextField

{

var newTxt:TextField = new TextField(); newTxt.x = x;

newTxt.y = y; newTxt.height = height;

newTxt.width = width; newTxt.border = true; newTxt.multiline = true; newTxt.wordWrap = true; newTxt.type = TextFieldType.INPUT; addChild(newTxt);

return newTxt;

}

public function onCopy(event:MouseEvent):void

{

Clipboard.generalClipboard.clear(); Clipboard.generalClipboard.setDataHandler(ClipboardFormats.TEXT_FORMAT,

renderData);

}

public function onPaste(event:Event):void

{

sourceTextField.text = Clipboard.generalClipboard.getData(ClipboardFormats.TEXT_FORMAT).toString;

}

public function renderData():String

{

trace(&quot;Rendering data&quot;);

var sourceStr:String = sourceTextField.text; if (sourceTextField.selectionEndIndex &gt;

sourceTextField.selectionBeginIndex)

{

return sourceStr.substring(sourceTextField.selectionBeginIndex,

sourceTextField.selectionEndIndex);

}

else

{

return sourceStr;

}

}

}

}

Flex example

&lt;mx:Application [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) layout=&quot;absolute&quot; width=&quot;326&quot; height=&quot;330&quot; applicationComplete=&quot;init()&quot;&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import openfl.desktop.Clipboard;

import openfl.desktop.ClipboardFormats;

public function init():void

{

destination.addEventListener(&quot;paste&quot;, doPaste);

}

public function doCopy():void

{

Clipboard.generalClipboard.clear(); Clipboard.generalClipboard.setDataHandler(ClipboardFormats.TEXT_FORMAT, renderData);

}

public function doPaste(event:Event):void

{

destination.text = Clipboard.generalClipboard.getData(ClipboardFormats.TEXT_FORMAT).toString;

}

public function renderData():String{ trace(&quot;Rendering data&quot;);

return source.text;

}

]]&gt;

&lt;/mx:Script&gt;

&lt;mx:Label x=&quot;10&quot; y=&quot;10&quot; text=&quot;Source&quot;/&gt;

&lt;mx:TextArea id=&quot;source&quot; x=&quot;10&quot; y=&quot;36&quot; width=&quot;300&quot; height=&quot;100&quot;&gt;

&lt;mx:text&gt;Neque porro quisquam est qui dolorem ipsum quia dolor sit amet, consectetur, adipisci velit.&lt;/mx:text&gt;

&lt;/mx:TextArea&gt;

&lt;mx:Label x=&quot;10&quot; y=&quot;181&quot; text=&quot;Destination&quot;/&gt;

&lt;mx:TextArea id=&quot;destination&quot; x=&quot;12&quot; y=&quot;207&quot; width=&quot;300&quot; height=&quot;100&quot;/&gt;

&lt;mx:Button click=&quot;doCopy();&quot; x=&quot;91&quot; y=&quot;156&quot; label=&quot;Copy&quot;/&gt;

&lt;/mx:Application&gt;