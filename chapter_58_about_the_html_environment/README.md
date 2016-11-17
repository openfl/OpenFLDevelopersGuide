# Chapter 58: About the HTML environment {#chapter-58-about-the-html-environment}

Adobe AIR 1.0 and later

Adobe® AIR® uses [WebKit](http://www.webkit.org/) _(www.webkit.org_), also used by the Safari web browser, to parse, layout, and render HTML and JavaScript content. Using the AIR APIs in HTML content is optional. You can program in the content of an HTMLLoader object or HTML window entirely with HTML and JavaScript. Most existing HTML pages and applications should run with few changes (assuming they use HTML, CSS, DOM, and JavaScript features compatible with WebKit).

**Important:** New versions of the Adobe AIR runtime may include updated versions of WebKit. A WebKit update in a new version of AIR _may_ result in unexpected changes in a deployed AIR application. These changes may affect the behavior or appearance of HTML content in an application. For example, improvements or corrections in WebKit rendering may change the layout of elements in an application’s user interface. For this reason, it is highly recommended that you provide an update mechanism in your application. Should you need to update your application due to a change in the WebKit version included in AIR, the AIR update mechanism can prompt the user to install the new version of your application.

The following table lists the version of the Safari web browser that uses the version of WebKit equivalent to that used in AIR:

| **AIR version** | **Safari version** |
| --- | --- |
| 1.0 | 2.04 |
| 1.1 | 3.04 |
| 1.5 | 4.0 Beta |
| 2.0 | 4.03 |
| 2.5 | 4.03 |
| 2.6 | 4.03 |
| 2.7 | 4.03 |
| 3 | 5.0.3 |

You can always determine the installed version of WebKit by examining the default user agent string returned by a HTMLLoader object:

var htmlLoader:HTMLLoader = new HTMLLoader(); trace( htmlLoader.userAgent );

Keep in mind that the version of WebKit used in AIR is not identical to the open source version. Some features are not supported in AIR and the AIR version can include security and bug fixes not yet available in the corresponding WebKit version. See

“WebKit features not supported in AIR” on page 977

.

Because AIR applications run directly on the desktop, with full access to the file system, the security model for HTML content is more stringent than the security model of a typical web browser. In AIR, only content loaded from the application installation directory is placed in the _application sandbox_. The application sandbox has the highest level of privilege and allows access to the AIR APIs. AIR places other content into isolated sandboxes based on where that content came from. Files loaded from the file system go into a local sandbox. Files loaded from the network using the http: or https: protocols go into a sandbox based on the domain of the remote server. Content in these non-application sandboxes is prohibited from accessing any AIR API and runs much as it would in a typical web browser.

HTML content in AIR does not display SWF or PDF content if alpha, scaling, or transparency settings are applied. For more information, see

“Considerations when loading SWF or PDF content in an HTML page” on page 1004

and

“Window transparency” on page 895

.

**More Help topics**

[Webkit DOM Reference](http://developer.apple.com/safari/library/documentation/AppleApplications/Reference/WebKitDOMRef/index.html%23//apple_ref/doc/uid/TP40006089) [Safari HTML Reference](http://developer.apple.com/safari/library/documentation/AppleApplications/Reference/SafariHTMLRef/Introduction.html) [Safari CSS Reference](http://developer.apple.com/safari/library/documentation/AppleApplications/Reference/SafariCSSRef/Introduction.html) [www.webkit.org](http://www.webkit.org/)

**Overview of the HTML environment**

Adobe AIR 1.0 and later

Adobe AIR provides a complete browser-like JavaScript environment with an HTML renderer, document object model, and JavaScript interpreter. The JavaScript environment is represented by the AIR HTMLLoader class. In HTML windows, an HTMLLoader object contains all HTML content, and is, in turn, contained within a NativeWindow object. In SWF content, the HTMLLoader class, which extends the Sprite class, can be added to the display list of a stage like any other display object. The Adobe® ActionScript® 3.0 properties of the class are described in

“Scripting the AIR HTML Container” on page 1003

and also in the [_ActionScript 3.0 Reference for the Adobe Flash_](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/index.html)[_Platform_](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/index.html). In the Flex framework, the AIR HTMLLoader class is wrapped in a mx:HTML component. The mx:HTML component extends the UIComponent class, so it can be used directly with other Flex containers. The JavaScript environment within the mx:HTML component is otherwise identical.

**About the JavaScript environment and its relationship to the AIR host**

Adobe AIR 1.0 and later

The following diagram illustrates the relationship between the JavaScript environment and the AIR run-time environment. Although only a single native window is shown, an AIR application can contain multiple windows. (And a single window can contain multiple HTMLLoader objects.)

_The JavaScript environment has its own Document and Window objects. JavaScript code can interact with the AIR run-time environment through the runtime, nativeWindow, and htmlLoader properties. ActionScript code can interact with the JavaScript environment through the window property of an HTMLLoader object, which is a reference to the JavaScript Window object. In addition, both ActionScript and JavaScript objects can listen for events dispatched by both AIR and JavaScript objects._

The runtime property provides access to AIR API classes, allowing you to create new AIR objects as well as access class (also called static) members. To access an AIR API, you add the name of the class, with package, to the runtime property. For example, to create a File object, you would use the statement:

var file = new window.runtime.filesystem.File();

**_Note:_ **_The AIR SDK provides a JavaScript file, AIRAliases.js, that defines more convenient aliases for the most commonly used AIR classes. When you import this file, you can use the shorter form air.Class instead of window.runtime.package.Class. For example, you could create the File object with new air.File()._

The NativeWindow object provides properties for controlling the desktop window. From within an HTML page, you can access the containing NativeWindow object with the window.nativeWindow property.

The HTMLLoader object provides properties, methods, and events for controlling how content is loaded and rendered. From within an HTML page, you can access the parent HTMLLoader object with the window.htmlLoader property.

**_Important:_ **_Only pages installed as part of an application have the htmlLoader, nativeWindow, or runtime properties and only when loaded as the top-level document. These properties are not added when a document is loaded into a frame or iframe. (A child document can access these properties on the parent document as long as it is in the same security sandbox. For example, a document loaded in a frame could access the runtime property of its parent with parent.runtime.)_

### About security {#about-security}

Adobe AIR 1.0 and later

AIR executes all code within a security sandbox based on the domain of origin. Application content, which is limited to content loaded from the application installation directory, is placed into the _application_ sandbox. Access to the run- time environment and the AIR APIs are only available to HTML and JavaScript running within this sandbox. At the same time, most dynamic evaluation and execution of JavaScript is blocked in the application sandbox after all handlers for the page load event have returned.

You can map an application page into a non-application sandbox by loading the page into a frame or iframe and setting the AIR-specific sandboxRoot and documentRoot attributes of the frame. By setting the sandboxRoot value to an actual remote domain, you can enable the sandboxed content to cross-script content in that domain. Mapping pages in this way can be useful when loading and scripting remote content, such as in a _mash-up_ application.

Another way to allow application and non-application content to cross-script each other, and the only way to give non- application content access to AIR APIs, is to create a _sandbox bridge_. A _parent-to-child_ bridge allows content in a child frame, iframe, or window to access designated methods and properties defined in the application sandbox. Conversely, a _child-to-parent_ bridge allows application content to access designated methods and properties defined in the sandbox of the child. Sandbox bridges are established by setting the parentSandboxBridge and childSandboxBridge properties of the window object. For more information, see

“HTML security in Adobe AIR” on page 1080

and

“HTML

frame and iframe elements” on page 973

.

### About plug-ins and embedded objects {#about-plug-ins-and-embedded-objects}

Adobe AIR 1.0 and later

AIR supports the Adobe® Acrobat® plug-in. Users must have Acrobat or Adobe® Reader® 8.1 (or better) to display PDF content. The HTMLLoader object provides a property for checking whether a user’s system can display PDF. SWF file content can also be displayed within the HTML environment, but this capability is built in to AIR and does not use an external plug-in.

No other WebKit plug-ins are supported in AIR.

**More Help topics**

“HTML security in Adobe AIR” on page 1080

“HTML Sandboxes” on page 966

“HTML frame and iframe elements” on page 973

“JavaScript Window object” on page 971

“The XMLHttpRequest object” on page 967

“Adding PDF content in AIR” on page 551