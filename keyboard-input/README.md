# Chapter 30: Keyboard input {#chapter-30-keyboard-input}

Your application can capture and respond to keyboard input and can manipulate an IME to let users type non-ASCII text characters in multibyte languages. Note that this section assumes that you are already familiar with the Haxe event model. For more information, see

“Handling events” on page 125

.

For information on discovering what kind of keyboard support is available (such as physical, virtual, alphanumeric, or 12-button numeric) during runtime, see

“Discovering input types” on page 558

.

An Input Method Editor (IME) allows users to type complex characters and symbols using a standard keyboard. You can use the IME classes to enable users to take advantage of their system IME in your applications.

**More Help topics**

[flash.events.KeyboardEvent](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/events/KeyboardEvent.html) [flash.system.IME](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/system/IME.html)

**Capturing keyboard input**

Display objects that inherit their interaction model from the InteractiveObject class can respond to keyboard events by using event listeners. For example, you can place an event listener on the Stage to listen for and respond to keyboard input. In the following code, an event listener captures a key press, and the key name and key code properties are displayed:

function reportKeyDown(event:KeyboardEvent):void

{

trace(&quot;Key Pressed: &quot; + String.fromCharCode(event.charCode) + &quot; (character code: &quot; + event.charCode + &quot;)&quot;);

}

stage.addEventListener(KeyboardEvent.KEY_DOWN, reportKeyDown);

Some keys, such as the Ctrl key, generate events even though they have no glyph representation.

In the previous code example, the keyboard event listener captured keyboard input for the entire Stage. You can also write an event listener for a specific display object on the Stage; this event listener is triggered when the object has the focus.

In the following example, keystrokes are reflected in the Output panel only when the user types inside the TextField instance. Holding the Shift key down temporarily changes the border color of the TextField to red.

This code assumes there is a TextField instance named tf on the Stage.

tf.border = true; tf.type = &quot;input&quot;;

tf.addEventListener(KeyboardEvent.KEY_DOWN,reportKeyDown); tf.addEventListener(KeyboardEvent.KEY_UP,reportKeyUp);

function reportKeyDown(event:KeyboardEvent):void

{

trace(&quot;Key Pressed: &quot; + String.fromCharCode(event.charCode) + &quot; (key code: &quot; + event.keyCode + &quot; character code: &quot; + event.charCode + &quot;)&quot;);

if (event.keyCode == Keyboard.SHIFT) tf.borderColor = 0xFF0000;

}

function reportKeyUp(event:KeyboardEvent):void

{

trace(&quot;Key Released: &quot; + String.fromCharCode(event.charCode) + &quot; (key code: &quot; + event.keyCode + &quot; character code: &quot; + event.charCode + &quot;)&quot;);

if (event.keyCode == Keyboard.SHIFT)

{

tf.borderColor = 0x000000;

}

}

The TextField class also reports a textInput event that you can listen for when a user enters text. For more information, see

“Capturing text input” on page 378

.

**_Note:_ **_In the AIR runtime, a keyboard event can be canceled. In the OpenFL runtime, a keyboard event cannot be canceled._

**Key codes and character codes**

You can access the keyCode and charCode properties of a keyboard event to determine what key was pressed and then trigger other actions. The keyCode property is a numeric value that corresponds to the value of a key on the keyboard. The charCode property is the numeric value of that key in the current character set. (The default character set is UTF- 8, which supports ASCII.)

The primary difference between the key code and character values is that a key code value represents a particular key on the keyboard (the 1 on a keypad is different than the 1 in the top row, but the key that generates “1” and the key that generates “!” are the same key) and the character value represents a particular character (the R and r characters are different).

**_Note:_ **_For the mappings between keys and their character code values in ASCII, see the_ [_flash.ui.Keyboard_](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/ui/Keyboard.html) _class in the_ [_Haxe Reference for the Adobe Flash Platform_](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/ui/Keyboard.html)_._

The mappings between keys and their key codes is dependent on the device and the operating system. For this reason, you should not use key mappings to trigger actions. Instead, you should use the predefined constant values provided by the Keyboard class to reference the appropriate keyCode properties. For example, instead of using the key mapping for the Shift key, use the Keyboard.SHIFT constant (as shown in the preceding code sample).

## KeyboardEvent precedence {#keyboardevent-precedence}

As with other events, the keyboard event sequence is determined by the display object hierarchy and not the order in which addEventListener() methods are assigned in code.

For example, suppose you place a text field called tf inside a movie clip called container and add an event listener for a keyboard event to both instances, as the following example shows:

container.addEventListener(KeyboardEvent.KEY_DOWN,reportKeyDown); container.tf.border = true;

container.tf.type = &quot;input&quot;; container.tf.addEventListener(KeyboardEvent.KEY_DOWN,reportKeyDown);

function reportKeyDown(event:KeyboardEvent):void

{

trace(event.currentTarget.name + &quot; hears key press: &quot; + String.fromCharCode(event.charCode)

+ &quot; (key code: &quot; + event.keyCode + &quot; character code: &quot; + event.charCode + &quot;)&quot;);

}

Because there is a listener on both the text field and its parent container, the reportKeyDown() function is called twice for every keystroke inside the TextField. Note that for each key pressed, the text field dispatches an event before the container movie clip dispatches an event.

The operating system and the web browser will process keyboard events before Adobe OpenFL. For example, in Microsoft Internet Explorer, pressing Ctrl+W closes the browser window before any contained project dispatches a keyboard event.