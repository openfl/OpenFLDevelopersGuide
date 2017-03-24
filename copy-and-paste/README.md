# Chapter 33: Copy and paste {#chapter-33-copy-and-paste}

OpenFL 10 and later, Adobe AIR 1.0 and later

Use the classes in the clipboard API to copy information to and from the system clipboard. The data formats that can be transferred into or out of an application running in Adobe® Flash® Player or Adobe® AIR® include:

• Text

• HTML-formatted text

• Rich Text Format data

• Serialized objects

• Object references (valid only within the originating application)

• Bitmaps (AIR only)

• Files (AIR only)

• URL strings (AIR only)

**Basics of copy-and-paste**

OpenFL 10 and later, Adobe AIR 1.0 and later

The copy-and-paste API contains the following classes.

| **Package** | **Classes** |
| --- | --- |
| flash.desktop | 

*   [Clipboard](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/desktop/Clipboard.html)
*   [ClipboardFormats](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/desktop/ClipboardFormats.html)
*   [ClipboardTransferMode](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/desktop/ClipboardTransferMode.html)

 |

The static Clipboard.generalClipboard property represents the operating system clipboard. The Clipboard class provides methods for reading and writing data to clipboard objects.

The HTMLLoader class (in AIR) and the TextField class implement default behavior for the normal copy and paste keyboard shortcuts. To implement copy and paste shortcut behavior for custom components, you can listen for these keystrokes directly. You can also use native menu commands along with key equivalents to respond to the keystrokes indirectly.

Different representations of the same information can be made available in a single Clipboard object to increase the ability of other applications to understand and use the data. For example, an image might be included as image data, a serialized Bitmap object, and as a file. Rendering of the data in a format can be deferred so that the format is not actually created until the data in that format is read.