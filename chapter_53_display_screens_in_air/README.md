# Chapter 53: Display screens in AIR {#chapter-53-display-screens-in-air}

Adobe AIR 1.0 and later

Use the Adobe® AIR® Screen class to access information about the display screens attached to a computer or device.

**More Help topics**

[flash.display.Screen](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Screen.html)

**Basics of display screens in AIR**

Adobe AIR 1.0 and later

• [Measuring the virtual desktop](http://www.adobe.com/go/learn_air_qs_virtualdesktop_en) (Flex)

• [Measuring the virtual desktop](http://www.adobe.com/go/learn_air_qs_virtualdesktop_flash_en) (Flash)

The screen API contains a single class, Screen, which provides static members for getting system screen information, and instance members for describing a particular screen.

A computer system can have several monitors or displays attached, which can correspond to several desktop screens arranged in a virtual space. The AIR Screen class provides information about the screens, their relative arrangement, and their usable space. If more than one monitor maps to the same screen, only one screen exists. If the size of a screen is larger than the display area of the monitor, there is no way to determine which portion of the screen is currently visible.

A screen represents an independent desktop display area. Screens are described as rectangles within the virtual desktop. The upper-left corner of screen designated as the primary display is the origin of the virtual desktop coordinate system. All values used to describe a screen are provided in pixels.

Screen bounds Virtual screen Usable bounds

_In this screen arrangement, two screens exist on the virtual desktop. The coordinates of the upper-left corner of the main screen (#1) are always (0,0). If the screen arrangement is changed to designate screen #2 as the main screen, then the coordinates of screen #1 become negative._

_Menubars, taskbars, and docks are excluded when reporting the usable bounds for a screen._

For detailed information about the screen API class, methods, properties, and events, see the [ActionScript 3.0](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Screen.html) [Reference for the Adobe Flash Platform](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/display/Screen.html).