# Chapter 51: Working with AIR runtime and operating system information {#chapter-51-working-with-air-runtime-and-operating-system-information}

Adobe AIR 1.0 and later

This section discusses ways that an AIR application can manage operating system file associations, detect user activity, and get information about the Adobe® AIR® runtime.

**More Help topics**

[flash.desktop.NativeApplication](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/desktop/NativeApplication.html)

**Managing file associations**

Adobe AIR 1.0 and later

Associations between your application and a file type must be declared in the application descriptor. During the installation process, the AIR application installer associates the AIR application as the default opening application for each of the declared file types, unless another application is already the default. The AIR application install process does not override an existing file type association. To take over the association from another application, call the NativeApplication.setAsDefaultApplication() method at run time.

It is a good practice to verify that the expected file associations are in place when your application starts up. This is because the AIR application installer does not override existing file associations, and because file associations on a user’s system can change at any time. When another application has the current file association, it is also a polite practice to ask the user before taking over an existing association.

The following methods of the NativeApplication class let an application manage file associations. Each of the methods takes the file type extension as a parameter:

| **Method** | **Description** |
| --- | --- |
| isSetAsDefaultApplication() | Returns true if the AIR application is currently associated with the specified file type. |
| setAsDefaultApplication() | Creates the association between the AIR application and the open action of the file type. |
| removeAsDefaultApplication() | Removes the association between the AIR application and the file type. |
| getDefaultApplication() | Reports the path of the application that is currently associated with the file type. |

AIR can only manage associations for the file types originally declared in the application descriptor. You cannot get information about the associations of a non-declared file type, even if a user has manually created the association between that file type and your application. Calling any of the file association management methods with the extension for a file type not declared in the application descriptor causes the application to throw a runtime exception.