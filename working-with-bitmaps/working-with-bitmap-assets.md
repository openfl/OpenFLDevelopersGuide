# Working with bitmap assets {#working-with-bitmap-assets}

Using bitmap assets, instead of loading bitmaps from a web URL, a file system
path, or generating the bitmap data manually, is another possibility for
rendering bitmap images. In some cases, bitmap assets will be embedded in the
generated binary, and in others, they will be included as separate files.
However, the API for working with either option is the same, allowing you to
write code once and target many platforms.

## Specifying bitmap assets

In a
[Lime _project.xml_ file](https://lime.openfl.org/docs/project-files/xml-format/),
include bitmap image files using the
[`<assets>` element](https://lime.openfl.org/docs/project-files/xml-format/#assets).
Bitmap assets may be of the `bitmap` type only.

- image

```xml
<assets path="assets">
	<image path="img/MyImage.png" id="MyImage" />
	<image path="img/AnotherImage.jpg" />
</assets>
```

## Using bitmap assets

The following code works with a bitmap asset with the id "MyImage". The code
gets an instance of the bitmap asset and passes it to a new Bitmap display
object:

```haxe
import openfl.utils.Assets;
import openfl.display.Bitmap;
import openfl.display.BitmapData;
import openfl.display.Sprite;

class BitmapAssetExample extends Sprite
{
	public function new()
	{
		super();
		var bmd:BitmapData = Assets.getBitmapData("MyImage");
		addChild(new Bitmap(bmd));
	}
}
```

## More Help topics

- [openfl.utils.Assets class](https://api.openfl.org/openfl/utils/Assets.html)
- [project.xml `<assets>` element](https://lime.openfl.org/docs/project-files/xml-format/#assets)
