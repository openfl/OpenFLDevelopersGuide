# Chapter 29: Basics of user interaction {#chapter-29-basics-of-user-interaction}

Your application can create interactivity by using Haxe to respond to user activity. Note that this section assumes that you are already familiar with the Haxe event model. For more information, see

"Handling

events" on page 125

.

**Capturing user input**

User interaction, whether by keyboard, mouse, camera, or a combination of these devices, is the foundation of interactivity. In Haxe, identifying and responding to user interaction primarily involves listening to events.

The InteractiveObject class, a subclass of the DisplayObject class, provides the common structure of events and functionality necessary for handling user interaction. You do not directly create an instance of the InteractiveObject class. Instead, display objects such as SimpleButton, Sprite, TextField, and various Flash authoring tool and Flex components inherit their user interaction model from this class and therefore share a common structure. This means that the techniques you learn and the code you write to handle user interaction in an object derived from InteractiveObject are applicable to all the others.

Important concepts and terms

It’s important to familiarize yourself with the following key user interaction terms before proceeding:

**Character code** A numeric code representing a character in the current character set (associated with a key being pressed on the keyboard). For example, "D" and "d" have different character codes even though they’re created by the same key on a U.S. English keyboard.

**Context menu** The menu that appears when a user right-clicks or uses a particular keyboard-mouse combination. Context menu commands typically apply directly to what has been clicked. For example, a context menu for an image may contain a command to show the image in a separate window and a command to download it.

**Focus** The indication that a selected element is active and that it is the target of keyboard or mouse interaction.

**Key code** A numeric code corresponding to a physical key on the keyboard.

**More Help topics**

[InteractiveObject](https://api.openfl.org/openfl/display/InteractiveObject.html) [Keyboard](https://api.openfl.org/openfl/ui/Keyboard.html)

[Mouse](https://api.openfl.org/openfl/ui/Mouse.html) [ContextMenu](https://api.openfl.org/openfl/ui/ContextMenu.html)