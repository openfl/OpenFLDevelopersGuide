# Working with sound assets {#working-with-sound-assets}

Using sound assets, instead of loading sounds from a web URL or a file system
path, is another possibility for playing audio. In some cases sound assets will
be embedded in the generated binary, and in others, they will be included as
separate files. However, the API for working with either option is the same,
allowing you to write code once and target many platforms.

## Specifying sound assets

In a
[Lime _project.xml_ file](https://lime.openfl.org/docs/project-files/xml-format/),
include sound files using the
[`<assets>` element](https://lime.openfl.org/docs/project-files/xml-format/#assets).
Sounds assets may be of one of the following types:

- music
- sound

> Some targets can only support playing one music file at a time. You should use
> "music" for files which are designed to play as background music, and "sound"
> for all other audio.

```xml
<assets path="assets">
	<sound path="sound/MySound.wav" id="MySound" />
	<music path="sound/BackgroundMusic.ogg" />
</assets>
```

## Using sound assts

The following code works with a sound asset with the id "MySound". The code get
an instance of the sound asset and calls the `play()` method on that instance:

```haxe
import openfl.utils.Assets;
import openfl.display.Sprite;
import openfl.media.Sound;

class SoundAssetExample extends Sprite
{
	public function new()
	{
		super();
		var smallSound:Sound = Assets.getSound("MySound");
		smallSound.play();
	}
}
```

## More Help topics

- [openfl.utils.Assets class](https://api.openfl.org/openfl/utils/Assets.html)
- [project.xml `<assets>` element](https://lime.openfl.org/docs/project-files/xml-format/#assets)
