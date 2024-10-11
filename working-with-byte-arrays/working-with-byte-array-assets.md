# Working with byte array assets {#working-with-byte-array-assets}

Using byte array assets, instead of loading binary data from a web URL or a file
system pat is another possibility for reading byte arrays. In some cases, byte
array assets will be embedded in the generated binary, and in others, they will
be included as separate files. However, the API for working with either option
is the same, allowing you to write code once and target many platforms.

## Specifying byte array assets

In a
[Lime _project.xml_ file](https://lime.openfl.org/docs/project-files/xml-format/),
include binary files using the
[`<assets>` element](https://lime.openfl.org/docs/project-files/xml-format/#assets).
Byte array assets may be of the `binary` type only.

- image

```xml
<assets path="assets">
	<binary path="img/MyBytes.dat" id="MyBytes" />
	<binary path="img/AnotherFile.bin" />
</assets>
```

## Using byte array assets

The following code works with a byte array asset with the id "MyBytes". The code
gets an instance of the byte array asset and prints its length to the debug
console:

```haxe
import openfl.utils.Assets;
import openfl.display.Sprite;
import openfl.utils.ByteArray;

class BinaryAssetExample extends Sprite
{
	public function new()
	{
		super();
		var bytes:ByteArray = Assets.getBytes("MyBytes");
		trace(bytes.length);
	}
}
```

## More Help topics

- [openfl.utils.Assets class](https://api.openfl.org/openfl/utils/Assets.html)
- [project.xml `<assets>` element](https://lime.openfl.org/docs/project-files/xml-format/#assets)
