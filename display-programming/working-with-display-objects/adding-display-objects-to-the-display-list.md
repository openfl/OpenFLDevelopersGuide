# Adding display objects to the display list {#adding-display-objects-to-the-display-list}

When you instantiate a display object, it will not appear on-screen (on the Stage) until you add the display object instance to a display object container that is on the display list. For example, in the following code, the `myText` TextField object would not be visible if you omitted the last line of code. In the last line of code, the `this` keyword must refer to a display object container that is already added to the display list.

```haxe
import openfl.display.*;
import openfl.text.TextField;

...

var myText = new TextField ();
myText.text = "Buenos dias.";
this.addChild (myText);
```

When you add any visual element to the Stage, that element becomes a _child_ of the Stage object. The first project loaded in an application (for example, the one that you embed in an HTML page) is automatically added as a child of the Stage. It can be an object of any type that extends the Sprite class.

Any display objects that you create _without_ using Haxe&mdash;for example, by placing an item on the Stage in Adobe Animate&mdash;are added to the display list. Although you do not add these display objects through OpenFL, you can access them through Haxe. For example, the following code adjusts the width of an object named `button1` that was added in the authoring tool (not through OpenFL):

```haxe
button1.width = 200;
```