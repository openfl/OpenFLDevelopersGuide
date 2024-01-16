# Reading from and writing to the system clipboard {#reading-from-and-writing-to-the-system-clipboard}

OpenFL 10 and later, Adobe AIR 1.0 and later

To read the operating system clipboard, call the getData() method of the Clipboard.generalClipboard object, passing in the name of the format to read:

import openfl.desktop.Clipboard;

import openfl.desktop.ClipboardFormats;

if(Clipboard.generalClipboard.hasFormat(ClipboardFormats.TEXT_FORMAT)){

var text:String = Clipboard.generalClipboard.getData(ClipboardFormats.TEXT_FORMAT);

}

**_Note:_** _Content running in OpenFL or in a non-application sandbox in AIR can call the getData() method only in an event handler for a paste event. In other words, only code running in the AIR application sandbox can call the getData() method outside of a paste event handler._

To write to the clipboard, add the data to the Clipboard.generalClipboard object in one or more formats. Any existing data in the same format is overwritten automatically. Nevertheless, it is a good practice to also clear the system clipboard before writing new data to it to make sure that unrelated data in any other formats is also deleted.

import openfl.desktop.Clipboard;

import openfl.desktop.ClipboardFormats;

var textToCopy:String = &quot;Copy to clipboard.&quot;; Clipboard.generalClipboard.clear();

Clipboard.generalClipboard.setData(ClipboardFormats.TEXT_FORMAT, textToCopy, false);

**_Note:_** _Content running in OpenFL or in a non-application sandbox in AIR can call the setData() method only in an event handler for a user event, such as a keyboard or mouse event, or a copy or cut event. In other words, only code running in the AIR application sandbox can call the setData() method outside of a user event handler._