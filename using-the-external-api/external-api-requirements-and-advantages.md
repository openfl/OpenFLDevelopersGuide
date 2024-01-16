# External API requirements and advantages {#external-api-requirements-and-advantages}

The external API is the portion of Haxe that provides a mechanism for communication between Haxe and code running in an “external application” that is acting as a container for OpenFL (commonly a web browser or stand-alone projector application). In Haxe, the functionality of the external API is provided by the ExternalInterface class. In OpenFL versions prior to OpenFL 8, the fscommand() action was used to carry out communication with the container application. The ExternalInterface class is a replacement for fscommand().

**_Note:_** _If you need to use the old fscommand() function—for example, to maintain compatibility with older applications or to interact with a third-party SWF container application or the stand-alone OpenFL—it is still available as a package-level function in the openfl.system package._

The ExternalInterface class is a subsystem that lets you easily communicate from Haxe and OpenFL to JavaScript in an HTML page.

The ExternalInterface class is available only under the following conditions:

*   In all supported versions of Internet Explorer for Windows (5.0 and later)
*   In any browser that supports the NPRuntime interface, which currently includes Firefox 1.0 and later, Mozilla 1.7.5 and later, Netscape 8.0 and later, and Safari 1.3 and later.
*   In an AIR application when the SWF is embedded in an HTML page displayed by the HTMLLoader control.

In all other situations (such as running in a stand-alone player), the ExternalInterface.available property returns false.

From Haxe, you can call a JavaScript function on the HTML page. The external API offers the following improved functionality compared with fscommand():

*   You can use any JavaScript function, not only the functions that you can use with the fscommand() function.
*   You can pass any number of arguments, with any names; you aren&#039;t limited to passing a command and a single string argument. This gives the external API much more flexibility than fscommand().
*   You can pass various data types (such as Boolean, Number, and String); you aren’t limited to String parameters.
*   You can receive the value of a call, and that value returns immediately to Haxe (as the return value of the call you make).

**_Important:_ **_If the name given to the OpenFL instance in an HTML page (the object tag’s id attribute) includes a hyphen (-) or other characters that are defined as operators in JavaScript (such as +, *, /, \, ., and so on), ExternalInterface calls from Haxe will not work when the container web page is viewed in Internet Explorer.In addition, if the HTML tags that define the OpenFL instance (the object and embed tags) are nested in an HTML form tag, ExternalInterface calls from Haxe will not work._