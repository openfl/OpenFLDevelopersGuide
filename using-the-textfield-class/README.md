# Chapter 21: Using the TextField class {#chapter-21-using-the-textfield-class}

You can use an instance of the
[TextField class](https://api.openfl.org/openfl/text/TextField.html) to display
text or create a text input field on the screen in OpenFL. The TextField class
is the basis for other text-based components, such as the TextArea components or
the TextInput components in [Haxe UI](http://haxeui.org/) or
[Feathers UI](https://feathersui.com/).

Text field content can be pre-specified in Haxe code, loaded from a text file or
database, or entered by a user interacting with your application. Within a text
field, the text can appear as rendered HTML content, with images embedded in the
rendered HTML. After you create an instance of a text field, you can use
openfl.text classes, such as TextFormat and StyleSheet, to control the
appearance of the text. The
[openfl.text package](https://api.openfl.org/openfl/text/index.html) contains
nearly all the classes related to creating, managing, and formatting text in
OpenFL.

You can format text by defining the formatting with a TextFormat object and
assigning that object to the text field. If your text field contains HTML text,
you can apply a StyleSheet object to the text field to assign styles to specific
pieces of the text field content. The TextFormat object or StyleSheet object
contains properties defining the appearance of the text, such as color, size,
and weight. The TextFormat object assigns the properties to all the content
within a text field or to a range of text. For example, within the same text
field, one sentence can be bold red text and the next sentence can be blue
italic text.

In addition to the classes in the
[openfl.text package](https://api.openfl.org/openfl/text/index.html), you can
use the
[openfl.events.TextEvent class](https://api.openfl.org/openfl/events/TextEvent.html)
to respond to user actions related to text.

## Section Contents

- [Displaying text](./displaying-text.md)
- [Selecting and manipulating text](./selecting-and-manipulating-text.md)
- [Capturing text input](./capturing-text-input.md)
- [Restricting text input](./restricting-text-input.md)
- [Formatting text](./formatting-text.md)
- [Advanced text rendering](./advanced-text-rendering.md)
- [Working with static text](./working-with-static-text.md)
- [Working with font assets](./working-with-font-assets.md)

<!-- TODO: uncomment when this content is adapted for OpenFL
- [TextField Example: Newspaper-style text formatting](./textfield-example-newspaper-style-text-formatting.md)-->