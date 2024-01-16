# Capturing text input {#capturing-text-input}

By default, the [`type` property of a text field](https://api.openfl.org/openfl/text/TextField.html#type) is set to `dynamic`. If you set
the `type` property to `input` using the TextFieldType class, you can collect
user input and save the value for use in other parts of your application. Input
text fields are useful for forms and any application that wants the user to
define a text value for use elsewhere in the program.

For example, the following code creates an input text field called `myTextBox`.
As the user enters text in the field, the `textInput` event is triggered. An
event handler called `textInputCapture` captures the string of text entered and
assigns it a variable. OpenFL displays the new text in another text field,
called `myOutputBox`.

```haxe
import openfl.display.Sprite;
import openfl.text.*;
import openfl.events.*;

class CaptureUserInput extends Sprite
{
	private var myTextBox:TextField = new TextField();
	private var myOutputBox:TextField = new TextField();
	private var myText:String = "Type your text here.";

	public function new()
	{
		super();
		captureText();
	}

	function captureText():Void
	{
		myTextBox.type = TextFieldType.INPUT;
		myTextBox.background = true;
		addChild(myTextBox);
		myTextBox.text = myText;
		myTextBox.addEventListener(TextEvent.TEXT_INPUT, textInputCapture);
	}

	function textInputCapture(event:TextEvent):Void
	{
		var str:String = myTextBox.text;
		createOutputBox(str);
	}

	function createOutputBox(str:String):Void
	{
		myOutputBox.background = true;
		myOutputBox.x = 200;
		addChild(myOutputBox);
		myOutputBox.text = str;
	}
}
```
