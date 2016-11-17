## Invoking an AIR application from the browser {#invoking-an-air-application-from-the-browser}

Adobe AIR 1.0 and later

Using the browser invocation feature, a web site can launch an installed AIR application to be launched from the browser. Browser invocation is only permitted if the application descriptor file sets allowBrowserInvocation to true:

&lt;allowBrowserInvocation&gt;true&lt;/allowBrowserInvocation&gt;

When the application is invoked via the browser, the application’s NativeApplication object dispatches a BrowserInvokeEvent object.

To receive BrowserInvokeEvent events, call the addEventListener() method of the NativeApplication object (NativeApplication.nativeApplication) in the AIR application. When an event listener registers for a BrowserInvokeEvent event, it also receives all BrowserInvokeEvent events that occurred before the registration. These events are dispatched after the call to addEventListener() returns, but not necessarily before other BrowserInvokeEvent events that might be received after registration. This allows you to handle BrowserInvokeEvent events that have occurred before your initialization code executes (such as when the application was initially invoked from the browser). Keep in mind that if you add an event listener later in execution (after application initialization) it still receives all BrowserInvokeEvent events that have occurred since the application started.

The BrowserInvokeEvent object includes the following properties:

| **Property** | **Description** |
| --- | --- |
| arguments | An array of arguments (strings) to pass to the application. |
| isHTTPS | Whether the content in the browser uses the https URL scheme (true) or not (false). |
| isUserEvent | Whether the browser invocation resulted in a user event (such as a mouse click). In AIR 1.0, this is always set to |
| sandboxType | The sandbox type for the content in the browser. Valid values are defined the same as those that can be used in the Security.sandboxType property, and can be one of the following: |
| securityDomain | The security domain for the content in the browser, such as [&quot;www.adobe.com&quot;](http://www.adobe.com/) or [&quot;www.example.org&quot;](http://www.example.org/). This property is only set for content in the remote security sandbox (for content from a network domain). It is not set for content in a local or application security sandbox. |

If you use the browser invocation feature, be sure to consider security implications. When a web site launches an AIR application, it can send data via the arguments property of the BrowserInvokeEvent object. Be careful using this data in any sensitive operations, such as file or code loading APIs. The level of risk depends on what the application is doing with the data. If you expect only a specific web site to invoke the application, the application should check the securityDomain property of the BrowserInvokeEvent object. You can also require the web site invoking the application to use HTTPs, which you can verify by checking the isHTTPS property of the BrowserInvokeEvent object.

The application should validate the data passed in. For example, if an application expects to be passed URLs to a specific domain, it should validate that the URLs really do point to that domain. This can prevent an attacker from tricking the application into sending it sensitive data.

No application should use BrowserInvokeEvent arguments that might point to local resources. For example, an application should not create File objects based on a path passed from the browser. If remote paths are expected to be passed from the browser, the application should ensure that the paths do not use the file:// protocol instead of a remote protocol.