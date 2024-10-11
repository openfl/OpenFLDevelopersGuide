# Chapter 33: Copy and paste {#chapter-33-copy-and-paste}

Use the classes in the clipboard API to copy information to and from the system
clipboard. The data formats that can be transferred into or out of an
application running in OpenFL include:

- Text

<!-- TODO: uncomment if this content is adapted for OpenFL

- HTML-formatted text

- Rich Text Format data

- Serialized objects

- Object references (valid only within the originating application)

- Bitmaps (AIR only)

- Files (AIR only)

- URL strings (AIR only)

-->

> HTML text, rich text, bitmaps, and other more advanced formats may be
> supported in future versions.

The static `Clipboard.generalClipboard` property represents the operating system
clipboard. The Clipboard class provides methods for reading and writing data to
clipboard objects.

The TextField class implements default behavior for the normal copy and paste
keyboard shortcuts. To implement copy and paste shortcut behavior for custom
components, you can listen for these keystrokes directly. You can also use
native menu commands along with key equivalents to respond to the keystrokes
indirectly.

<!-- TODO: uncomment if this content is adapted for OpenFL
Different representations of the same information can be made available in a
single Clipboard object to increase the ability of other applications to
understand and use the data. For example, an image might be included as image
data, a serialized Bitmap object, and as a file. Rendering of the data in a
format can be deferred so that the format is not actually created until the data
in that format is read.
-->

## Section Contents

- [Reading from and writing to the system clipboard](./reading-from-and-writing-to-the-system-clipboard.md)

## More Help Topics

- [Clipboard](https://api.openfl.org/openfl/desktop/Clipboard.html)
- [ClipboardFormats](https://api.openfl.org/openfl/desktop/ClipboardFormats.html)
- [ClipboardTransferMode](https://api.openfl.org/openfl/desktop/ClipboardTransferMode.html)
