# Chapter 21: Using the TextField class {#chapter-21-using-the-textfield-class}

You can use an instance of the TextField class to display text or create a text input field on the screen in Adobe® Flash® Player or Adobe® AIR™. The TextField class is the basis for other text-based components, such as the TextArea components or the TextInput components.

Text field content can be pre-specified in the project, loaded from a text file or database, or entered by a user interacting with your application. Within a text field, the text can appear as rendered HTML content, with images embedded in the rendered HTML. After you create an instance of a text field, you can use openfl.text classes, such as TextFormat and StyleSheet, to control the appearance of the text. The [openfl.text package](https://api.openfl.org/openfl/text/index.html) contains nearly all the classes related to creating, managing, and formatting text in Haxe.

You can format text by defining the formatting with a TextFormat object and assigning that object to the text field. If your text field contains HTML text, you can apply a StyleSheet object to the text field to assign styles to specific pieces of the text field content. The TextFormat object or StyleSheet object contains properties defining the appearance of the text, such as color, size, and weight. The TextFormat object assigns the properties to all the content within a text field or to a range of text. For example, within the same text field, one sentence can be bold red text and the next sentence can be blue italic text.

In addition to the classes in the openfl.text package, you can use the openfl.events.TextEvent class to respond to user actions related to text.

**More Help topics**

"Assigning text formats" on page 380

"Displaying HTML text" on page 375

"Applying cascading style sheets" on page 380

**Displaying text**

Although authoring tools like Adobe Flash Builder and Flash Professional provide several options for displaying text, including text-related components or text tools, the simplest way to display text programmatically is through a text field.

**Types of text**

The type of text within a text field is characterized by its source:

*   Dynamic text

Dynamic text includes content that is loaded from an external source, such as a text file, an XML file, or even a remote web service.

*   Input text

Input text is any text entered by a user or dynamic text that a user can edit. You can set up a style sheet to format input text, or use the openfl.text.TextFormat class to assign properties to the text field for the input content. For more information, see

"Capturing text input" on page 378

.

*   Static text

Static text is created through Flash Professional only. You cannot create a static text instance using Haxe

3.0\. However, you can use Haxe classes like StaticText and TextSnapshot to manipulate an existing static text instance. For more information, see

"Working with static text" on page 386

.

## Modifying the text field contents {#modifying-the-text-field-contents}

You can define dynamic text by assigning a string to the [openfl.text.TextField.text](https://api.openfl.org/openfl/text/TextField.html#text) property. You assign a string directly to the property, as follows:

myTextField.text = "Hello World";

You can also assign the text property a value from a variable defined in your script, as in the following example:

package

{

import openfl.display.Sprite;
import openfl.text.*;

public class TextWithImage extends Sprite

{

private var myTextBox:TextField = new TextField();
private var myText:String = "Hello World";

public function TextWithImage()

{

addChild(myTextBox);
myTextBox.text = myText;

}

}

}

Alternatively, you can assign the text property a value from a remote variable. You have three options for loading text values from remote sources:

*   The openfl.net.URLLoader and openfl.net.URLRequest classes load variables for the text from a local or remote location.
*   The FlashVars attribute is embedded in the HTML page hosting the project and can contain values for text variables.
*   The openfl.net.SharedObject class manages persistent storage of values. For more information, see

    "Storing local

    data" on page 701

    .

## Displaying HTML text {#displaying-html-text}

The openfl.text.TextField class has an htmlText property that you can use to identify your text string as one containing HTML tags for formatting the content. As in the following example, you must assign your string value to the htmlText property (not the text property) for OpenFL to render the text as HTML:

var myText:String = "&lt;p&gt;This is &lt;b&gt;some&lt;/b&gt; content to &lt;i&gt;render&lt;/i&gt; as &lt;u&gt;HTML&lt;/u&gt; text.&lt;/p&gt;"; myTextBox.htmlText = myText;

OpenFL support a subset of HTML tags and entities for the htmlText property. The [openfl.text.TextField.htmlText property description in the OpenFL API Reference](https://api.openfl.org/openfl/text/TextField.html#htmlText) provides detailed information about the supported HTML tags and entities.

Once you designate your content using the htmlText property, you can use style sheets or the textformat tag to manage the formatting of your content. For more information, see

"Formatting text" on page 380

.

## Using images in text fields {#using-images-in-text-fields}

Another advantage to displaying your content as HTML text is that you can include images in the text field. You can reference an image, local or remote, using the img tag and have it appear within the associated text field.

The following example creates a text field named myTextBox and includes a JPG image of an eye, stored in the same directory as the project, within the displayed text:

package

{

import openfl.display.Sprite;
import openfl.text.*;

public class TextWithImage extends Sprite

{

private var myTextBox:TextField;

private var myText:String = "&lt;p&gt;This is &lt;b&gt;some&lt;/b&gt; content to &lt;i&gt;test&lt;/i&gt; and

&lt;i&gt;see&lt;/i&gt;&lt;/p&gt;&lt;p&gt;&lt;img src='eye.jpg' width='20' height='20'&gt;&lt;/p&gt;&lt;p&gt;what can be rendered.&lt;/p&gt;&lt;p&gt;You should see an eye image and some &lt;u&gt;HTML&lt;/u&gt; text.&lt;/p&gt;";

public function TextWithImage()

{

myTextBox.width = 200;

myTextBox.height = 200; myTextBox.multiline = true; myTextBox.wordWrap = true; myTextBox.border = true;

addChild(myTextBox); myTextBox.htmlText = myText;

}

}

}

The img tag supports JPEG, GIF, PNG, and projects.

## Scrolling text in a text field {#scrolling-text-in-a-text-field}

In many cases, your text can be longer than the text field displaying the text. Or you may have an input field that allows a user to input more text than can be displayed at one time. You can use the scroll-related properties of the openfl.text.TextField class to manage lengthy content, either vertically or horizontally.

The scroll-related properties include TextField.scrollV, TextField.scrollH and maxScrollV and maxScrollH. Use these properties to respond to events, like a mouse click or a keypress.

The following example creates a text field that is a set size and contains more text than the field can display at one time. As the user clicks the text field, the text scrolls vertically.

package

{

import openfl.display.Sprite;
import openfl.text.*;

import openfl.events.MouseEvent;

public class TextScrollExample extends Sprite

{

private var myTextBox:TextField = new TextField();

private var myText:String = "Hello world and welcome to the show. It's really nice to meet you. Take your coat off and stay a while. OK, show is over. Hope you had fun. You can go home now. Don't forget to tip your waiter. There are mints in the bowl by the door. Thank you. Please come again.";

public function TextScrollExample()

{

myTextBox.text = myText; myTextBox.width = 200;

myTextBox.height = 50; myTextBox.multiline = true; myTextBox.wordWrap = true; myTextBox.background = true; myTextBox.border = true;

var format:TextFormat = new TextFormat(); format.font = "Verdana";

format.color = 0xFF0000; format.size = 10;

myTextBox.defaultTextFormat = format; addChild(myTextBox);

myTextBox.addEventListener(MouseEvent.MOUSE_DOWN, mouseDownScroll);

}

public function mouseDownScroll(event:MouseEvent):void

{

myTextBox.scrollV++;

}

}

}