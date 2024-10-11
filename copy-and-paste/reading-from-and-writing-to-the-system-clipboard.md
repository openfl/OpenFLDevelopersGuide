# Reading from and writing to the system clipboard

To read the operating system clipboard, call the `getData()` method of the
`Clipboard.generalClipboard` object, passing in the name of the format to read:

```haxe
import openfl.desktop.Clipboard;
import openfl.desktop.ClipboardFormats;
import openfl.display.Sprite;

class ClipboardReadExample extends Sprite {
	public function new() {
		super();

		if (Clipboard.generalClipboard.hasFormat(ClipboardFormats.TEXT_FORMAT)) {
			var text:String = Clipboard.generalClipboard.getData(ClipboardFormats.TEXT_FORMAT);
		}
	}
}
```

> **Note:** Content running in some sandboxed OpenFL targets can call the
> `getData()` method only in an event handler for a `paste` event.

To write to the clipboard, add the data to the `Clipboard.generalClipboard`
object in one or more formats. Any existing data in the same format is
overwritten automatically. Nevertheless, it is a good practice to also clear the
system clipboard before writing new data to it to make sure that unrelated data
in any other formats is also deleted.

```haxe
import openfl.desktop.Clipboard;
import openfl.desktop.ClipboardFormats;
import openfl.display.Sprite;

class ClipboardWriteExample extends Sprite {
	public function new() {
		super();

		var textToCopy:String = "Copy to clipboard.";
		Clipboard.generalClipboard.clear();
		Clipboard.generalClipboard.setData(ClipboardFormats.TEXT_FORMAT, textToCopy, false);
	}
}
```

> **Note:** Content running in some sandboxed OpenFL targets can call the
> `setData()` method only in an event handler for a user event, such as a
> keyboard or mouse event, or a `copy` or `cut` event.
