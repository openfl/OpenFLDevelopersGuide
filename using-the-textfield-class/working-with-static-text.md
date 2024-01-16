# Working with static text {#working-with-static-text}

Static text is created only within Adobe Animate (formerly named Adobe Flash
Professional). You cannot programmatically instantiate static text using Haxe
code. Static text is useful if the text is short and is not intended to change
(as dynamic text can). Think of static text as similar to a graphic element like
a circle or square drawn on the Stage in Adobe Animate. While static text is
more limited than dynamic text, OpenFL does allow you to read the property
values of static text using the [StaticText class](https://api.openfl.org/openfl/text/StaticText.html).

<!-- TODO: uncomment if TextSnapshot is implemented
You can also use the TextSnapshot class to read values out of the static text. -->

## Accessing static text fields with the StaticText class

Typically, you use the [openfl.text.StaticText class](https://api.openfl.org/openfl/text/StaticText.html) to interact with a static
text instance placed on the Stage. You can't instantiate a static text instance
programmatically. Static text is created in Adobe Animate.

To create a reference to an existing static text field, iterate over the items
in the display list and assign a variable. For example:

```haxe
for (i in 0...this.numChildren; i++) {
    var displayitem:DisplayObject = this.getChildAt(i);
    if (Std.isOfType(displayitem, StaticText)) {
        trace("a static text field is item " + i + " on the display list");
        var myFieldLabel:StaticText = cast(displayitem, StaticText);
        trace("and contains the text: " + myFieldLabel.text);
    }
}
```

Once you have a reference to a static text field, you can use the properties of
that text field in Haxe. The following code assumes that a variable named
`myFieldLabel` is assigned to a static text reference. A dynamic text field
named `myField` is positioned relative to the `x` and `y` values of
`myFieldLabel` and displays the value of `myFieldLabel` again.

```haxe
var myField:TextField = new TextField();
addChild(myField);
myField.x = myFieldLabel.x;
myField.y = myFieldLabel.y + 20;
myField.autoSize = TextFieldAutoSize.LEFT;
myField.text = "and " + myFieldLabel.text
```

<!-- TODO: uncomment if TextSnapshot is implemented
## Using the TextSnapshot class

If you want to programmatically work with an existing static text instance, you
can use the openfl.text.TextSnapshot class to work with the `textSnapshot`
property of a openfl.display.DisplayObjectContainer. In other words, you create a
TextSnapshot instance from the `DisplayObjectContainer.textSnapshot` property.
You can then apply methods to that instance to retrieve values or select parts
of the static text.

For example, place a static text field that contains the text "TextSnapshot
Example" on the Stage. Add the following Haxe code:

```haxe
var mySnap:TextSnapshot = this.textSnapshot;
var count:Number = mySnap.charCount;
mySnap.setSelected(0, 4, true);
mySnap.setSelected(1, 2, false);
var myText:String = mySnap.getSelectedText(false);
trace(myText);
```

The TextSnapshot class is useful for getting the text out of static text fields
in a loaded SWF file, if you want to use the text as a value in another part of
an application.
