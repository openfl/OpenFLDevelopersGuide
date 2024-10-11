# Working with font assets {#working-with-font-assets}

Using font assets allows custom fonts to be included with your OpenFL project. In some cases, font assets will be embedded in the
generated binary, and in others, they will be included as separate files.
However, the API for working with either option is the same, allowing you to
write code once and target many platforms.

## Specifying font assets

In a
[Lime _project.xml_ file](https://lime.openfl.org/docs/project-files/xml-format/),
include font image files using the
[`<assets>` element](https://lime.openfl.org/docs/project-files/xml-format/#assets).
Font assets may be of the `font` type only.

- font

```xml
<assets path="assets">
	<font path="fnt/MyFont.ttf" id="MyFont" />
	<font path="fnt/AnotherFont.otf" />
</assets>
```

## Using font assets

The following code works with a font asset with the id "fnt". The code gets
an instance of the font asset and uses it with a TextField:

```haxe
import openfl.utils.Assets;
import openfl.display.Sprite;
import openfl.text.Font;
import openfl.text.TextFormat;
import openfl.text.TextField;

class FontAssetExample extends Sprite
{
	public function new()
	{
		super();
		var fnt:Font = Assets.getFont("MyFont");
		var tf:TextField = new TextField();
		tf.defaultTextFormat = new TextFormat(fnt.getName());
		tf.text = "Hello world";
		addChild(tf);
	}
}
```

## More Help topics

- [openfl.utils.Assets class](https://api.openfl.org/openfl/utils/Assets.html)
- [project.xml `<assets>` element](https://lime.openfl.org/docs/project-files/xml-format/#assets)
