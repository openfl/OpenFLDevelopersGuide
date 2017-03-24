# Discovering input types {#discovering-input-types}

OpenFL 10.1 and later, Adobe AIR 2 and later

The OpenFL 10.1 and Adobe AIR 2 releases introduced the ability to test the runtime environment for support of specific input types. You can use Haxe to test if the device on which the runtime is currently deployed:

• Supports stylus or finger input (or no touch input at all).

• Has a virtual or physical keyboard for the user (or no keyboard at all).

• Displays a cursor (if not, then features that are dependent upon having a cursor hover over an object do not work). The input discovery Haxe APIs include:

• [flash.system.Capabilities.touchscreenType property](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/system/Capabilities.html#touchscreenType): A value provided at runtime indicating what input type is supported in the current environment.

• [flash.system.TouchscreenType class](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/system/TouchscreenType.html): A class of enumeration value constants for the Capabilities.touchscreenType property.

• [flash.ui.Mouse.supportsCursor property](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/ui/Mouse.html#supportsCursor): A value provided at runtime indicating if a persistent cursor is available or not.

• [flash.ui.Keyboard.physicalKeyboardType property](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/ui/Keyboard.html#physicalKeyboardType): A value provided at runtime indicating if a full physical keyboard is available or a numeric keypad, only, or no keyboard at all.

• [flash.ui.KeyboardType class](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/ui/KeyboardType.html): A class of enumeration value constants for the flash.ui.Keyboard.physicalKeyboardType property.

• [flash.ui.Keyboard.hasVirtualKeyboard property](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/ui/Keyboard.html#hasVirtualKeyboard): A value provided at runtime indicating if a virtual keyboard is provided to the user (either in place of a physical keyboard, or in addition to a physical keyboard).

The input discovery APIs let you take advantage of a user’s device capabilities, or provide alternatives when those capabilities are not present. These API are especially useful for developing mobile and touch-enabled applications. For example, if you have an interface for a mobile device that has small buttons for a stylus, you can provide an alternative interface with larger buttons for a user using finger touches for input. The following code is for an application that has a function called createStylusUI() that assigns one set of user interface elements appropriate for stylus interaction.

Another function, called createTouchUI(), assigns another set of user interface elements appropriate for finger interaction:

if(Capabilities.touchscreenType == TouchscreenType.STYLUS ){

//Construct the user interface using small buttons for a stylus

//and allow more screen space for other visual content createStylusUI();

} else if(Capabilities.touchscreenType = TouchscreenType.FINGER){

//Construct the user interface using larger buttons

//to capture a larger point of contact with the device createTouchUI();

}

When developing applications for different input environments, consider the following compatibility chart:

| **Environment** | **supportsCursor** | **touchscreenType == FINGER** | **touchscreenType == STYLUS** | **touchscreenType == NONE** |
| --- | --- | --- | --- | --- |
| Traditional Desktop | true | false | false | true |
| Capacitive Touchscreen Devices (tablets, PDAs, and phones that detect subtle human touch, such as the Apple iPhone or Palm Pre) | false | true | false | false |
| Resistive Touchscreen devices (tablets, PDAs, and phonesthat detect precise, high-pressure contact, such as the HTC Fuze) | false | false | true | false |
| Non-Touchscreen devices (feature phones and devices that run applications but don’t have screens that detect contact) | false | false | false | true |

**_Note:_ **_Different device platforms can support many combinations of input types. Use this chart as a general guide._