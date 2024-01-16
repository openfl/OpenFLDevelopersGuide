# Restricting text input {#restricting-text-input}

Since input text fields are often used for forms or dialog boxes in
applications, you may want to limit the types of characters a user can enter in
a text field, or even keep the text hidden — for example, for a password. The
openfl.text.TextField class has a
[`displayAsPassword` property](https://api.openfl.org/openfl/text/TextField.html#displayAsPassword)
and a [`restrict` property](https://api.openfl.org/openfl/text/TextField.html#restrict)
that you can set to control user input.

The `displayAsPassword` property simply hides the text (displaying it as a
series of asterisks) as the user types it. When `displayAsPassword` is set to
`true`, the Cut and Copy commands and their corresponding keyboard shortcuts do
not function. As the following example shows, you assign the `displayAsPassword`
property just as you would other properties, such as background and color:

```haxe
myTextBox.type = TextFieldType.INPUT;
myTextBox.background = true;
myTextBox.displayAsPassword = true;
addChild(myTextBox);
```

The `restrict` property is a little more complicated since you must specify
which characters the user is allowed to type in an input text field. You can
allow specific letters, numbers, or ranges of letters, numbers, and characters.
The following code allows the user to enter only uppercase letters (and not
numbers or special characters) in the text field:

```haxe
myTextBox.restrict = "A-Z";
```

OpenFL uses hyphens to define ranges, and carets to define excluded characters.
For more information about defining what is restricted in an input text field,
see the
[`openfl.text.TextField.restrict` property](https://api.openfl.org/openfl/text/TextField.html#restrict)
entry in the OpenFL API Reference.

Note: If you use the `openfl.text.TextField.restrict` property, the runtime
automatically converts restricted letters to the allowed case.
