# Working with static text {#working-with-static-text}

Static text is created only within Flash Professional. You cannot programmatically instantiate static text using Haxe. Static text is useful if the text is short and is not intended to change (as dynamic text can). Think of static text as similar to a graphic element like a circle or square drawn on the Stage in Flash Professional. While static text is more limited than dynamic text, Haxe does allow you to read the property values of static text using the StaticText class. You can also use the TextSnapshot class to read values out of the static text.

**Accessing static text fields with the StaticText class**

Typically, you use the flash.text.StaticText class in the Actions panel of Flash Professional to interact with a static text instance placed on the Stage. You may also work in Haxe files that interact with a project containing static text. In either case, you can’t instantiate a static text instance programmatically. Static text is created in Flash Professional.

To create a reference to an existing static text field, iterate over the items in the display list and assign a variable. For example:

for (var i = 0; i &lt; this.numChildren; i++) {

var displayitem:DisplayObject = this.getChildAt(i); if (displayitem instanceof StaticText) {

trace(&quot;a static text field is item &quot; + i + &quot; on the display list&quot;); var myFieldLabel:StaticText = StaticText(displayitem); trace(&quot;and contains the text: &quot; + myFieldLabel.text);

}

}

Once you have a reference to a static text field, you can use the properties of that field in Haxe\. The following code is attached to a frame in the Timeline, and assumes that a variable named myFieldLabel is assigned to a static text reference. A dynamic text field named myField is positioned relative to the x and y values of myFieldLabel and displays the value of myFieldLabel again.

var myField:TextField = new TextField(); addChild(myField);

myField.x = myFieldLabel.x; myField.y = myFieldLabel.y + 20;

myField.autoSize = TextFieldAutoSize.LEFT; myField.text = &quot;and &quot; + myFieldLabel.text

## Using the TextSnapshot class {#using-the-textsnapshot-class}

If you want to programmatically work with an existing static text instance, you can use the flash.text.TextSnapshot class to work with the textSnapshot property of a flash.display.DisplayObjectContainer. In other words, you create a TextSnapshot instance from the DisplayObjectContainer.textSnapshot property. You can then apply methods to that instance to retrieve values or select parts of the static text.

For example, place a static text field that contains the text &quot;TextSnapshot Example&quot; on the Stage. Add the following Haxe to Frame 1 of the Timeline:

var mySnap:TextSnapshot = this.textSnapshot; var count:Number = mySnap.charCount; mySnap.setSelected(0, 4, true);

mySnap.setSelected(1, 2, false);

var myText:String = mySnap.getSelectedText(false); trace(myText);

The TextSnapshot class is useful for getting the text out of static text fields in a loaded project, if you want to use the text as a value in another part of an application.