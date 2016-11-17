## AIR and WebKit {#air-and-webkit}

Adobe AIR 1.0 and later

Adobe AIR uses the open source WebKit engine, also used in the Safari web browser. AIR adds several extensions to allow access to the runtime classes and objects as well as for security. In addition, WebKit itself adds features not included in the W3C standards for HTML, CSS, and JavaScript.

Only the AIR additions and the most noteworthy WebKit extensions are covered here; for additional documentation on non-standard HTML, CSS, and JavaScript, see [www.webkit.org](http://www.webkit.org/) and [developer.apple.com](http://developer.apple.com/internet/safari/). For standards information, see the [W3C web site](http://www.w3.org/). Mozilla also provides a valuable [general reference](http://developer.mozilla.org/en/docs/Main_Page)on HTML, CSS, and DOM topics (of course, the WebKit and Mozilla engines are not identical).

**JavaScript in AIR**

Flash Player 9 and later, Adobe AIR 1.0 and later

AIR makes several changes to the typical behavior of common JavaScript objects. Many of these changes are made to make it easier to write secure applications in AIR. At the same time, these differences in behavior mean that some common JavaScript coding patterns, and existing web applications using those patterns, might not always execute as expected in AIR. For information on correcting these types of issues, see

“Avoiding security-related JavaScript errors”

on page 983

.

**HTML Sandboxes**

Adobe AIR 1.0 and later

AIR places content into isolated sandboxes according to the origin of the content. The sandbox rules are consistent with the same-origin policy implemented by most web browsers, as well as the rules for sandboxes implemented by the Adobe Flash Player. In addition, AIR provides a new _application_ sandbox type to contain and protect application content. See

“Security sandboxes” on page 1044

for more information on the types of sandboxes you may encounter when developing AIR applications.

Access to the run-time environment and AIR APIs are only available to HTML and JavaScript running within the application sandbox. At the same time, however, dynamic evaluation and execution of JavaScript, in its various forms, is largely restricted within the application sandbox for security reasons. These restrictions are in place whether or not your application actually loads information directly from a server. (Even file content, pasted strings, and direct user input may be untrustworthy.)

The origin of the content in a page determines the sandbox to which it is consigned. Only content loaded from the application directory (the installation directory referenced by the app: URL scheme) is placed in the application sandbox. Content loaded from the file system is placed in the _local-with-file system_ or the _local-trusted_ sandbox, which allows access and interaction with content on the local file system, but not remote content. Content loaded from the network is placed in a remote sandbox corresponding to its domain of origin.

To allow an application page to interact freely with content in a remote sandbox, the page can be mapped to the same domain as the remote content. For example, if you write an application that displays map data from an Internet service, the page of your application that loads and displays content from the service could be mapped to the service domain. The attributes for mapping pages into a remote sandbox and domain are new attributes added to the frame and iframe HTML elements.

To allow content in a non-application sandbox to safely use AIR features, you can set up a parent sandbox bridge. To allow application content to safely call methods and access properties of content in other sandboxes, you can set up a child sandbox bridge. Safety here means that remote content cannot accidentally get references to objects, properties, or methods that are not explicitly exposed. Only simple data types, functions, and anonymous objects can be passed across the bridge. However, you must still avoid explicitly exposing potentially dangerous functions. If, for example, you exposed an interface that allowed remote content to read and write files anywhere on a user’s system, then you might be giving remote content the means to do considerable harm to your users.

JavaScript eval() function

Adobe AIR 1.0 and later

Use of the eval() function is restricted within the application sandbox once a page has finished loading. Some uses are permitted so that JSON-formatted data can be safely parsed, but any evaluation that results in executable statements results in an error.

“Code restrictions for content in different sandboxes” on page 1082

describes the allowed uses of the eval() function.

Function constructors

Adobe AIR 1.0 and later

In the application sandbox, function constructors can be used before a page has finished loading. After all page load

event handlers have finished, new functions cannot be created.

Loading external scripts

Adobe AIR 1.0 and later

HTML pages in the application sandbox cannot use the script tag to load JavaScript files from outside of the application directory. For a page in your application to load a script from outside the application directory, the page must be mapped to a non-application sandbox.

The XMLHttpRequest object

Adobe AIR 1.0 and later

AIR provides an XMLHttpRequest (XHR) object that applications can use to make data requests. The following example illustrates a simple data request:

xmlhttp = new XMLHttpRequest();

xmlhttp.open(&quot;GET&quot;, [&quot;http:/www.example.com/file.data&quot;,](http://www.example.com/file.data) true); xmlhttp.onreadystatechange = function() {

if (xmlhttp.readyState == 4) {

//do something with data...

}

}

xmlhttp.send(null);

In contrast to a browser, AIR allows content running in the application sandbox to request data from any domain. The result of an XHR that contains a JSON string can be evaluated into data objects unless the result also contains executable code. If executable statements are present in the XHR result, an error is thrown and the evaluation attempt fails.

To prevent accidental injection of code from remote sources, synchronous XHRs return an empty result if made before a page has finished loading. Asynchronous XHRs will always return after a page has loaded.

By default, AIR blocks cross-domain XMLHttpRequests in non-application sandboxes. A parent window in the application sandbox can choose to allow cross-domain requests in a child frame containing content in a non- application sandbox by setting allowCrossDomainXHR, an attribute added by AIR, to true in the containing frame or iframe element:

&lt;iframe id=&quot;mashup&quot; [src=&quot;http://www.example.com/map.html&quot;](http://www.example.com/map.html) allowCrossDomainXHR=&quot;true&quot;

&lt;/iframe&gt;

**_Note:_ **_When convenient, the AIR URLStream class can also be used to download data._

If you dispatch an XMLHttpRequest to a remote server from a frame or iframe containing application content that has been mapped to a remote sandbox, make sure that the mapping URL does not mask the server address used in the XHR. For example, consider the following iframe definition, which maps application content into a remote sandbox for the example.com domain:

&lt;iframe id=&quot;mashup&quot; [src=&quot;http://www.example.com/map.html&quot;](http://www.example.com/map.html) documentRoot=&quot;app:/sandbox/&quot; [sandboxRoot=&quot;http://www.example.com/&quot;](http://www.example.com/) allowCrossDomainXHR=&quot;true&quot;

&lt;/iframe&gt;

Because the sandboxRoot attribute remaps the root URL of the [www.example.com](http://www.example.com/) address, all requests are loaded from the application directory and not the remote server. Requests are remapped whether they derive from page navigation or from an XMLHttpRequest.

To avoid accidentally blocking data requests to your remote server, map the sandboxRoot to a subdirectory of the remote URL rather than the root. The directory does not have to exist. For example, to allow requests to the [www.example.com](http://www.example.com/) to load from the remote server rather than the application directory, change the previous iframe to the following:

&lt;iframe id=&quot;mashup&quot; [src=&quot;http://www.example.com/map.html&quot;](http://www.example.com/map.html) documentRoot=&quot;app:/sandbox/&quot; [sandboxRoot=&quot;http://www.example.com/air/&quot;](http://www.example.com/air/) allowCrossDomainXHR=&quot;true&quot;

&lt;/iframe&gt;

In this case, only content in the air subdirectory is loaded locally.

For more information on sandbox mapping see

“HTML frame and iframe elements” on page 973

and

“HTML security

in Adobe AIR” on page 1080

.

Cookies

Adobe AIR 1.0 and later

In AIR applications, only content in remote sandboxes (content loaded from http: and https: sources) can use cookies (the document.cookie property). In the application sandbox, other means for storing persistent data are available, including the EncryptedLocalStore, SharedObject, and FileStream classes.

The Clipboard object

Adobe AIR 1.0 and later

The WebKit Clipboard API is driven with the following events: copy, cut, and paste. The event object passed in these events provides access to the clipboard through the clipboardData property. Use the following methods of the clipboardData object to read or write clipboard data:

| **Method** | **Description** |
| --- | --- |
| clearData(mimeType) | Clears the clipboard data. Set the mimeType parameter to the MIME type of the data to clear. |
| getData(mimeType) | Get the clipboard data. This method can only be called in a handler for the paste event. Set the mimeType |
| setData(mimeType, data) | Copy data to the clipboard. Set the mimeType parameter to the MIME type of the data. |

JavaScript code outside the application sandbox can only access the clipboard through theses events. However, content in the application sandbox can access the system clipboard directly using the AIR Clipboard class. For example, you could use the following statement to get text format data on the clipboard:

var clipping = air.Clipboard.generalClipboard.getData(&quot;text/plain&quot;,

air.ClipboardTransferMode.ORIGINAL_ONLY);

The valid data MIME types are:

| **MIME type** | **Value** |
| --- | --- |
| Text | &quot;text/plain&quot; |
| HTML | &quot;text/html&quot; |
| URL | &quot;text/uri-list&quot; |
| Bitmap | &quot;image/x-vnd.adobe.air.bitmap&quot; |
| File list | &quot;application/x-vnd.adobe.air.file-list&quot; |

**_Important:_ **_Only content in the application sandbox can access file data present on the clipboard. If non-application content attempts to access a file object from the clipboard, a security error is thrown._

For more information on using the clipboard, see

“Copy and paste” on page 596

and [Using the Pasteboard from](http://developer.apple.com/documentation/AppleApplications/Conceptual/SafariJSProgTopics/Tasks/CopyAndPaste.html%23//apple_ref/doc/uid/30001234) [JavaScript (Apple Developer Center)](http://developer.apple.com/documentation/AppleApplications/Conceptual/SafariJSProgTopics/Tasks/CopyAndPaste.html%23//apple_ref/doc/uid/30001234).

Drag and Drop

Adobe AIR 1.0 and later

Drag-and-drop gestures into and out of HTML produce the following DOM events: dragstart, drag, dragend, dragenter, dragover, dragleave, and drop. The event object passed in these events provides access to the dragged data through the dataTransfer property. The dataTransfer property references an object that provides the same methods as the clipboardData object associated with a clipboard event. For example, you could use the following function to get text format data from a drop event:

function onDrop(dragEvent){

return dragEvent.dataTransfer.getData(&quot;text/plain&quot;, air.ClipboardTransferMode.ORIGINAL_ONLY);

}

The dataTransfer object has the following important members:

| **Member** | **Description** |
| --- | --- |
| clearData(mimeType) | Clears the data. Set the mimeType parameter to the MIME type of the data representation to clear. |
| getData(mimeType) | Get the dragged data. This method can only be called in a handler for the drop event. Set the mimeType |
| setData(mimeType, data) | Set the data to be dragged. Set the mimeType parameter to the MIME type of the data. |
| types | An array of strings containing the MIME types of all data representations currently available in the |
| effectsAllowed | Specifies whether the data being dragged can be copied, moved, linked, or some combination thereof. Set the |
| dropEffect | Specifies which of the allowed drop effects are supported by a drag target. Set the dropEffect property in the handler for the dragEnter event. During the drag, the cursor changes to indicate which effect would occur if the user released the mouse. If no dropEffect is specified, an effectsAllowed property effect is chosen. The copy effect has priority over the move effect, which itself has priority over the link effect. The user can modify the default priority using the keyboard. |

For more information on adding support for drag-and-drop to an AIR application see

“Drag and drop in AIR” on

page 608

and [Using the Drag-and-Drop from JavaScript (Apple Developer Center)](http://developer.apple.com/documentation/AppleApplications/Conceptual/SafariJSProgTopics/Tasks/DragAndDrop.html%23//apple_ref/doc/uid/30001233).

innerHTML and outerHTML properties

Adobe AIR 1.0 and later

AIR places security restrictions on the use of the innerHTML and outerHTML properties for content running in the application sandbox. Before the page load event, as well as during the execution of any load event handlers, use of the innerHTML and outerHTML properties is unrestricted. However, once the page has loaded, you can only use innerHTML or outerHTML properties to add static content to the document. Any statement in the string assigned to innerHTML or outerHTML that evaluates to executable code is ignored. For example, if you include an event callback attribute in an element definition, the event listener is not added. Likewise, embedded &lt;script&gt; tags are not evaluated. For more information, see the

“HTML security in Adobe AIR” on page 1080

.

Document.write() and Document.writeln() methods

Adobe AIR 1.0 and later

Use of the write() and writeln() methods is not restricted in the application sandbox before the load event of the page. However, once the page has loaded, calling either of these methods does not clear the page or create a new one. In a non-application sandbox, as in most web browsers, calling document.write() or writeln() after a page has finished loading clears the current page and opens a new, blank one.

Document.designMode property

Adobe AIR 1.0 and later

Set the document.designMode property to a value of on to make all elements in the document editable. Built-in editor support includes text editing, copy, paste, and drag-and-drop. Setting designMode to on is equivalent to setting the contentEditable property of the body element to true. You can use the contentEditable property on most HTML elements to define which sections of a document are editable. See

“HTML contentEditable attribute” on

page 975

for additional information.

unload events (for body and frameset objects)

Adobe AIR 1.0 and later

In the top-level frameset or body tag of a window (including the main window of the application), do not use the unload event to respond to the window (or application) being closed. Instead, use exiting event of the NativeApplication object (to detect when an application is closing). Or use the closing event of the NativeWindow object (to detect when a window is closing). For example, the following JavaScript code displays a message (&quot;Goodbye.&quot;) when the user closes the application:

var app = air.NativeApplication.nativeApplication; app.addEventListener(air.Event.EXITING, closeHandler); function closeHandler(event)

{

alert(&quot;Goodbye.&quot;);

}

However, scripts _can_ successfully respond to the unload event caused by navigation of a frame, iframe, or top-level window content.

**_Note:_ **_These limitations may be removed in a future version of Adobe AIR._

JavaScript Window object

Adobe AIR 1.0 and later

The Window object remains the global object in the JavaScript execution context. In the application sandbox, AIR adds new properties to the JavaScript Window object to provide access to the built-in classes of AIR, as well as important host objects. In addition, some methods and properties behave differently depending on whether they are within the application sandbox or not.

**Window.runtime property** The runtime property allows you to instantiate and use the built-in runtime classes from within the application sandbox. These classes include the AIR and Flash Player APIs (but not, for example, the Flex framework). For example, the following statement creates an AIR file object:

var preferencesFile = new window.runtime.flash.filesystem.File();

The AIRAliases.js file, provided in the AIR SDK, contains alias definitions that allow you to shorten such references. For example, when AIRAliases.js is imported into a page, a File object can be created with the following statement:

var preferencesFile = new air.File();

The window.runtime property is only defined for content within the application sandbox and only for the parent document of a page with frames or iframes.

See

“Using the AIRAliases.js file” on page 988

.

**Window.nativeWindow property** The nativeWindow property provides a reference to the underlying native window object. With this property, you can script window functions and properties such as screen position, size, and visibility, and handle window events such as closing, resizing, and moving. For example, the following statement closes the window:

window.nativeWindow.close();

**_Note:_ **_The window control features provided by the NativeWindow object overlap the features provided by the JavaScript Window object. In such cases, you can use whichever method you find most convenient._

The window.nativeWindow property is only defined for content within the application sandbox and only for the parent document of a page with frames or iframes.

**Window.htmlLoader property** The htmlLoader property provides a reference to the AIR HTMLLoader object that contains the HTML content. With this property, you can script the appearance and behavior of the HTML environment. For example, you can use the htmlLoader.paintsDefaultBackground property to determine whether the control paints a default, white background:

window.htmlLoader.paintsDefaultBackground = false;

**_Note:_ **_The HTMLLoader object itself has a window property, which references the JavaScript Window object of the HTML content it contains. You can use this property to access the JavaScript environment through a reference to the containing HTMLLoader._

The window.htmlLoader property is only defined for content within the application sandbox and only for the parent document of a page with frames or iframes.

**Window.parentSandboxBridge and Window.childSandboxBridge properties** The parentSandboxBridge and childSandboxBridge properties allow you to define an interface between a parent and a child frame. For more information, see

“Cross-scripting content in different security sandboxes” on page 999

.

**Window.setTimeout() and Window.setInterval() functions** AIR places security restrictions on use of the setTimeout() and setInterval() functions within the application sandbox. You cannot define the code to be executed as a string when calling setTimeout() or setInterval(). You must use a function reference. For more information, see

“setTimeout() and setInterval()” on page 985

.

**Window.open() function** When called by code running in a non-application sandbox, the open() method only opens a window when called as a result of user interaction (such as a mouse click or keypress). In addition, the window title is prefixed with the application title (to prevent windows opened by remote content from impersonating windows opened by the application). For more information, see the [“Restrictions on calling the JavaScript window.open()](..\chapter_64_security\air_security.md#696154181379824-_bookmark704) [method” on page 1085](..\chapter_64_security\air_security.md#696154181379824-_bookmark704).

air.NativeApplication object

Adobe AIR 1.0 and later

The NativeApplication object provides information about the application state, dispatches several important application-level events, and provides useful functions for controlling application behavior. A single instance of the NativeApplication object is created automatically and can be accessed through the class-defined NativeApplication.nativeApplication property.

To access the object from JavaScript code you could use:

var app = window.runtime.flash.desktop.NativeApplication.nativeApplication;

Or, if the AIRAliases.js script has been imported, you could use the shorter form:

var app = air.NativeApplication.nativeApplication;

The NativeApplication object can only be accessed from within the application sandbox. For more information about the NativeApplication object, see

“Working with AIR runtime and operating system information” on page 888

.

The JavaScript URL scheme

Adobe AIR 1.0 and later

Execution of code defined in a JavaScript URL scheme (as in href=&quot;javascript:alert(&#039;Test&#039;)&quot;) is blocked within the application sandbox. No error is thrown.

### HTML in AIR {#html-in-air}

Adobe AIR 1.0 and later

AIR and WebKit define a couple of non-standard HTML elements and attributes, including:

“HTML frame and iframe elements” on page 973

“HTML element event handlers” on page 975

**HTML frame and iframe elements**

Adobe AIR 1.0 and later

AIR adds new attributes to the frame and iframe elements of content in the application sandbox:

**sandboxRoot attribute** The sandboxRoot attribute specifies an alternate, non-application domain of origin for the file specified by the frame src attribute. The file is loaded into the non-application sandbox corresponding to the specified domain. Content in the file and content loaded from the specified domain can cross-script each other.

**_Important:_ **_If you set the value of sandboxRoot to the base URL of the domain, all requests for content from that domain are loaded from the application directory instead of the remote server (whether that request results from page navigation, from an XMLHttpRequest, or from any other means of loading content)._

**documentRoot attribute** The documentRoot attribute specifies the local directory from which to load URLs that resolve to files within the location specified by sandboxRoot.

When resolving URLs, either in the frame src attribute, or in content loaded into the frame, the part of the URL matching the value specified in sandboxRoot is replaced with the value specified in documentRoot. Thus, in the following frame tag:

&lt;iframe [src=&quot;http://www.example.com/air/child.html&quot;](http://www.example.com/air/child.html) documentRoot=&quot;app:/sandbox/&quot; [sandboxRoot=&quot;http://www.example.com/air/&quot;/&gt;](http://www.example.com/air/)

child.html is loaded from the sandbox subdirectory of the application installation folder. Relative URLs in child.html are resolved based on sandbox directory. Note that any files on the remote server at [www.example.com/air](http://www.example.com/air) are not accessible in the frame, since AIR would attempt to load them from the app:/sandbox/ directory.

**allowCrossDomainXHR attribute** Include allowCrossDomainXHR=&quot;allowCrossDomainXHR&quot; in the opening frame tag to allow content in the frame to make XMLHttpRequests to any remote domain. By default, non-application content can only make such requests to its own domain of origin. There are serious security implications involved in allowing cross-domain XHRs. Code in the page is able to exchange data with any domain. If malicious content is somehow injected into the page, any data accessible to code in the current sandbox can be compromised. Only enable cross-domain XHRs for pages that you create and control and only when cross-domain data loading is truly necessary. Also, carefully validate all external data loaded by the page to prevent code injection or other forms of attack.

**_Important:_ **_If the allowCrossDomainXHR attribute is included in a frame or iframe element, cross-domain XHRs are enabled (unless the value assigned is &quot;0&quot; or starts with the letters &quot;f&quot; or &quot;n&quot;). For example, setting allowCrossDomainXHR to &quot;deny&quot; would still enable cross-domain XHRs. Leave the attribute out of the element declaration altogether if you do not want to enable cross-domain requests._

**ondominitialize attribute** Specifies an event handler for the dominitialize event of a frame. This event is an AIR- specific event that fires when the window and document objects of the frame have been created, but before any scripts have been parsed or document elements created.

The frame dispatches the dominitialize event early enough in the loading sequence that any script in the child page can reference objects, variables, and functions added to the child document by the dominitialize handler. The parent page must be in the same sandbox as the child to directly add or access any objects in a child document.

However, a parent in the application sandbox can establish a sandbox bridge to communicate with content in a non- application sandbox.

The following examples illustrate use of the iframe tag in AIR:

Place child.html in a remote sandbox, without mapping to an actual domain on a remote server:

&lt;iframe [src=&quot;http://localhost/air/child.html&quot;](http://localhost/air/child.html) documentRoot=&quot;app:/sandbox/&quot; [sandboxRoot=&quot;http://localhost/air/&quot;/&gt;](http://localhost/air/)

Place child.html in a remote sandbox, allowing XMLHttpRequests only to [www.example.com:](http://www.example.com/)

&lt;iframe [src=&quot;http://www.example.com/air/child.html&quot;](http://www.example.com/air/child.html) documentRoot=&quot;app:/sandbox/&quot; [sandboxRoot=&quot;http://www.example.com/air/&quot;/&gt;](http://www.example.com/air/)

Place child.html in a remote sandbox, allowing XMLHttpRequests to any remote domain:

&lt;iframe [src=&quot;http://www.example.com/air/child.html&quot;](http://www.example.com/air/child.html) documentRoot=&quot;app:/sandbox/&quot; [sandboxRoot=&quot;http://www.example.com/air/&quot;](http://www.example.com/air/) allowCrossDomainXHR=&quot;allowCrossDomainXHR&quot;/&gt;

Place child.html in a local-with-file-system sandbox:

&lt;iframe src=&quot;file:///templates/child.html&quot; documentRoot=&quot;app:/sandbox/&quot; sandboxRoot=&quot;app-storage:/templates/&quot;/&gt;

Place child.html in a remote sandbox, using the dominitialize event to establish a sandbox bridge:

&lt;html&gt;

&lt;head&gt;

&lt;script&gt;

var bridgeInterface = {}; bridgeInterface.testProperty = &quot;Bridge engaged&quot;; function engageBridge(){

document.getElementById(&quot;sandbox&quot;).parentSandboxBridge = bridgeInterface;

}

&lt;/script&gt;

&lt;/head&gt;

&lt;body&gt;

&lt;iframe id=&quot;sandbox&quot;

[src=&quot;http://www.example.com/air/child.html&quot;](http://www.example.com/air/child.html) documentRoot=&quot;app:/&quot; [sandboxRoot=&quot;http://www.example.com/air/&quot;](http://www.example.com/air/) ondominitialize=&quot;engageBridge()&quot;/&gt;

&lt;/body&gt;

&lt;/html&gt;

The following child.html document illustrates how child content can access the parent sandbox bridge:

&lt;html&gt;

&lt;head&gt;

&lt;script&gt;

document.write(window.parentSandboxBridge.testProperty);

&lt;/script&gt;

&lt;/head&gt;

&lt;body&gt;&lt;/body&gt;

&lt;/html&gt;

For more information, see

“Cross-scripting content in different security sandboxes” on page 999

and

“HTML security

in Adobe AIR” on page 1080

.

HTML element event handlers

Adobe AIR 1.0 and later

DOM objects in AIR and WebKit dispatch some events not found in the standard DOM event model. The following table lists the related event attributes you can use to specify handlers for these events:

| **Callback attribute name** | **Description** |
| --- | --- |
| oncontextmenu | Called when a context menu is invoked, such as through a right-click or command-click on selected text. |
| oncopy | Called when a selection in an element is copied. |
| oncut | Called when a selection in an element is cut. |
| ondominitialize | Called when the DOM of a document loaded in a frame or iframe is created, but before any DOM elements are created or scripts parsed. |
| ondrag | Called when an element is dragged. |
| ondragend | Called when a drag is released. |
| ondragenter | Called when a drag gesture enters the bounds of an element. |
| ondragleave | Called when a drag gesture leaves the bounds of an element. |
| ondragover | Called continuously while a drag gesture is within the bounds of an element. |
| ondragstart | Called when a drag gesture begins. |
| ondrop | Called when a drag gesture is released while over an element. |
| onerror | Called when an error occurs while loading an element. |
| oninput | Called when text is entered into a form element. |
| onpaste | Called when an item is pasted into an element. |
| onscroll | Called when the content of a scrollable element is scrolled. |
| onselectstart | Called when a selection begins. |

HTML contentEditable attribute

Adobe AIR 1.0 and later

You can add the contentEditable attribute to any HTML element to allow users to edit the content of the element. For example, the following example HTML code sets the entire document as editable, except for first p element:

&lt;html&gt;

&lt;head/&gt;

&lt;body contentEditable=&quot;true&quot;&gt;

&lt;h1&gt;de Finibus Bonorum et Malorum&lt;/h1&gt;

&lt;p contentEditable=&quot;false&quot;&gt;Sed ut perspiciatis unde omnis iste natus error.&lt;/p&gt;

&lt;p&gt;At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis.&lt;/p&gt;

&lt;/body&gt;

&lt;/html&gt;

**_Note:_ **_If you set the document.designMode property to on, then all elements in the document are editable, regardless of the setting of contentEditable for an individual element. However, setting designMode to off, does not disable editing of elements for which contentEditable is true. See_

_“Document.designMode property” on page 970_

_for additional information._

Data: URLs

Adobe AIR 2 and later

AIR supports data: URLs for the following elements:

• img

• input type=”image”

• CSS rules allowing images (such as background-image)

Data URLs allow you to insert binary image data directly into a CSS or HTML document as a base64-encoded string. The following example uses a data: URL as a repeating background:

&lt;html&gt;

&lt;head&gt;

&lt;style&gt; body { background-

image:url(&#039;data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAABkCAMAAABHPGVmAAAAGXRFWHRTb2Z 0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAAZQTFRF%2F6cA%2F%2F%2F%2Fgxp3lwAAAAJ0Uk5T%2FwDltzBKAAA

BF0lEQVR42uzZQQ7CMAxE0e%2F7X5oNCyRocWzPiJbMBZ6qpIljE%2BnwklgKG7kwUjc2IkIaxkY0CPdEsCCasws6ShX BgmBBmEagpXQQLAgWBAuSY2gaKaWPYEGwIEwg0FRmECwIFoQeQjJlhJWUEFazjFDJCkI5WYRWMgjtfEGYyQnCXD4jTCd m1zmngFpBFznwVNi5RPSbwbWnpYr%2BBHi%2FtCTfgPLEPL7jBctAKBRptXJ8M%2BprIuZKu%2BUKcg4YK1PLz7kx4bS qHyPaT4d%2B28OCJJiRBo4FCQsSA0bziT3XubMgYUG6fc5fatmGBQkL0hoJ1IaZMiQsSFiQ8vRscTjlQOI2iHZwtpHuf

%2BJAYiOiJSkj8Z%2FIQ4ABANvXGLd3%2BZMrAAAAAElFTkSuQmCC&#039;);

background-repeat:repeat;

}

&lt;/style&gt;

&lt;/head&gt;

&lt;body&gt;

&lt;/body&gt;

&lt;/html&gt;

When using data: URLS, be aware that extra whitespace is significant. For example, the data string must be entered as a single, unbroken line. Otherwise, the line breaks are treated as part of the data and the image cannot be decoded.

### CSS in AIR {#css-in-air}

Adobe AIR 1.0 and later

WebKit supports several extended CSS properties. Many of these extensions use the prefix: -webkit. Note that some of these extensions are experimental in nature and may be removed from a future version of WebKit. For more information about the Webkit support for CSS and its extensions to CSS, see [Safari CSS Reference](http://developer.apple.com/safari/library/documentation/AppleApplications/Reference/SafariCSSRef/Introduction.html).

### WebKit features not supported in AIR {#webkit-features-not-supported-in-air}

Adobe AIR 1.0 and later

AIR does not support the following features available in WebKit or Safari 4:

• Cross-domain messaging via window.postMessage (AIR provides its own cross-domain communication APIs)

• CSS variables

• Web Open Font Format (WOFF) and SVG fonts.

• HTML video and audio tags

• Media device queries

• Offline application cache

• Printing (AIR provides its own PrintJob API)

• Spelling and grammar checkers

• SVG

• WAI-ARIA

• WebSockets (AIR provides its own socket APIs)

• Web workers

• WebKit SQL API (AIR provides its own API)

• WebKit geolocation API (AIR provides its own geolocation API on supported devices)

• WebKit multi-file upload API

• WebKit touch events (AIR provides its own touch events)

• Wireless Markup Language (WML)

The following lists contain specific JavaScript APIs, HTML elements, and CSS properties and values that AIR does not support:

Unsupported JavaScript Window object members:

• applicationCache()

• console

• openDatabase()

• postMessage()

• document.print()

Unsupported HTML tags:

• audio

• video

Unsupported HTML attributes:

• aria-*

• draggable

• formnovalidate

• list

• novalidate

• onbeforeload

• onhashchange

• onorientationchange

• onpagehide

• onpageshow

• onpopstate

• ontouchstart

• ontouchmove

• ontouchend

• ontouchcancel

• onwebkitbeginfullscreen

• onwebkitendfullscreen

• pattern

• required

• sandbox

Unsupported JavaScript events:

• beforeload

• hashchange

• orientationchange

• pagehide

• pageshow

• popstate

• touchstart

• touchmove

• touchend

• touchcancel

• webkitbeginfullscreen

• webkitendfullscreen

Unsupported CSS properties:

• background-clip

• background-origin (use -webkit-background-origin)

• background-repeat-x

• background-repeat-y

• background-size (use -webkit-background-size)

• border-bottom-left-radius

• border-bottom-right-radius

• border-radius

• border-top-left-radius

• border-top-right-radius

• text-rendering

• -webkit-animation-play-state

• -webkit-background-clip

• -webkit-color-correction

• -webkit-font-smoothing

Unsupported CSS values:

• appearance property values:

• media-volume-slider-container

• media-volume-slider

• media-volume-sliderthumb

• outer-spin-button

• border-box (background-clip and background-origin)

• contain (background-size)

• content-box (background-clip and background-origin)

• cover (background-size)

• list property values:

• afar

• amharic

• amharic-abegede

• cjk-earthly-branch

• cjk-heavenly-stem

• ethiopic

• ethiopic-abegede

• ethiopic-abegede-am-et

• ethiopic-abegede-gez

• ethiopic-abegede-ti-er

• ethiopic-abegede-ti-et

• ethiopic-halehame-aa-er

• ethiopic-halehame-aa-et

• ethiopic-halehame-am-et

• ethiopic-halehame-gez

• ethiopic-halehame-om-et

• ethiopic-halehame-sid-et

• ethiopic-halehame-so-et

• ethiopic-halehame-ti-er

• ethiopic-halehame-ti-et

• ethiopic-halehame-tig

• hangul

• hangul-consonant

• lower-norwegian

• oromo

• sidama

• somali

• tigre

• tigrinya-er

• tigrinya-er-abegede

• tigrinya-et

• tigrinya-et-abegede

• upper-greek

• upper-norwegian

• -wap-marquee (display property)