# Specifying loading context {#specifying-loading-context}

When you load an external file into OpenFL through the `load()` or `loadBytes()` method of the Loader class, you can optionally specify a `context` parameter. This parameter is a LoaderContext object.

The LoaderContext class includes three properties that let you define the context of how the loaded content can be used:

*   `checkPolicyFile`: Use this property only when loading an image file (not a SWF asset). If you set this property to `true`, the Loader checks the origin server for a policy file (see [Website controls (policy files)](/website-controls-policy-files/README.md)). This is necessary only for content originating from domains other than that of the project containing the Loader object. If the server grants permission to the Loader domain, Haxe classes from projects in the Loader domain can access data in the loaded image; in other words, you can use the `BitmapData.draw()` command to access data in the loaded image.

Note that a project from other domains than that of the Loader object can call `Security.allowDomain()` to permit a specific domain.

*   `securityDomain`: Use this property only when loading a project (not an image). Specify this for a project from a domain other than that of the file containing the Loader object. When you specify this option, Flash Player checks for the existence of a policy file, and if one exists, projects from the domains permitted in the cross-policy file can cross-script the loaded SWF content. You can specify openfl.system.SecurityDomain.currentDomain as this parameter.
*   `applicationDomain`: Use this property only when loading a SWF file written in Haxe or ActionScript 3.0 (not an image or a SWF file written in ActionScript 1.0 or 2.0). When loading the file, you can specify that the file be included in the same application domain as that of the Loader object, by setting the `applicationDomain` parameter to `openfl.system.ApplicationDomain.currentDomain`. By putting the loaded project in the same application domain, you can access its classes directly. This can be useful if you are loading a project that contains embedded media, which you can access via their associated class names. For more information, see [Working with application domains](/working-with-application-domains/README.md).

Here’s an example of checking for a policy file when loading a bitmap from another domain:

```haxe
var context = new LoaderContext ();
context.checkPolicyFile = true;
var urlReq = new URLRequest ("http://www.[your_domain_here].com/photo11.jpg");
var ldr = new Loader ();
ldr.load (urlReq, context);
```

Here’s an example of checking for a policy file when loading a SWF from another domain, in order to place the file in the same security sandbox as the Loader object. Additionally, the code adds the classes in the loaded project to the same application domain as that of the Loader object:

```haxe
var context = new LoaderContext ();
context.securityDomain = SecurityDomain.currentDomain;
context.applicationDomain = ApplicationDomain.currentDomain;
var urlReq = new URLRequest ("http://www.[your_domain_here].com/library.swf");
var ldr = new Loader ();
ldr.load (urlReq, context);
```

For more information, see the [LoaderContext](http://api.openfl.org/openfl/system/LoaderContext.html) class in the [API Reference](http://api.openfl.org/openfl/system/LoaderContext.html).

<!--
## Loading projects in AIR for iOS {#loading-swf-files-in-air-for-ios}

Adobe AIR 3.6 and later, iOS only

On iOS devices, there are restrictions on loading and compiling code at runtime. Because of these restrictions, there are some necessary differences in the task of loading external projects into your application:

*   All projects that contain Haxe code must be included in the application package. No SWF containing code can be loaded from an external source such as over a network. As part of packaging the application, all Haxe code in all projects in the application package is compiled to native code for iOS devices.
*   You can’t load, unload, and then re-load a project. If you attempt to do this, an error occurs.
*   The behavior of loading into memory and then unloading it is the same as with desktop platforms. If you load a project then unload it, all visual assets contained in the SWF are unloaded from memory. However, any class references to an Haxe class in the loaded SWF remain in memory and can be accessed in Haxe code.
*   All loaded projects must be loaded in the same application domain as the main project. This is not the default behavior, so for each SWF you load you must create a LoaderContext object specifying the main application domain, and pass that LoaderContext object to the Loader.load() method call. If you attempt to load a SWF in an application domain other than the main SWF application domain, an error occurs. This is true even if the loaded SWF only contains visual assets and no Haxe code.

The following example shows the code to use to load a SWF from the application package into the main SWF’s application domain:

var loader:Loader = new Loader();

var url:URLRequest = new URLRequest(&quot;swfs/SecondarySwf.swf&quot;);

var loaderContext:LoaderContext = new LoaderContext(false, ApplicationDomain.currentDomain, null);

loader.load(url, loaderContext);

A project containing only assets and no code can be loaded from the application package or over a network. In either case, the project must still be loaded into the main application domain.

For AIR versions prior to AIR 3.6, all code is stripped from SWFs other than the main application SWF during the compilation process. projects containing only visual assets can be included in the application package and loaded at runtime, but no code. If you attempt to load a SWF that contains Haxe code, an error occurs. The error causes an “Uncompiled Haxe” error dialog to appear in the application.

See also

[Packaging and loading multiple SWFs in AIR apps on iOS](http://blogs.adobe.com/airodynamics/2012/11/09/packaging-and-loading-multiple-swfs-in-air-apps-on-ios/)

## Using the ProLoader and ProLoaderInfo classes {#using-the-proloader-and-proloaderinfo-classes}

OpenFL 9 and later, Adobe AIR 1.0 and later, and requires Flash Professional CS5.5

To help with remote shared library (RSL) preloading, Flash Professional CS5.5 introduces the fl.display.ProLoader and fl.display.ProLoaderInfo classes. These classes mirror the flash.display.Loader and flash.display.LoaderInfo classes but provide a more consistent loading experience.

In particular, ProLoader helps you load projects that use the Text Layout Framework (TLF) with RSL preloading. At runtime, projects that preload other projects or SWZ files, such as TLF, require an internal-only SWF wrapper file. The extra layer of complexity imposed by the SWF wrapper file can result in unwanted behavior. ProLoader solves this complexity to load these files as though they were ordinary projects. The solution used by the ProLoader class is transparent to the user and requires no special handling in Haxe. In addition, ProLoader loads ordinary SWF content correctly.

In Flash Professional CS5.5 and later, you can safely replace all usages of the Loader class with the ProLoader class. Then, export your application to OpenFL 10.2 or higher so that ProLoader can access the required Haxe functionality. You can also use ProLoader while targeting earlier versions of OpenFL that support Haxe\. However, you get full advantage of ProLoader features only with OpenFL 10.2 or higher. Always use ProLoader when you use TLF in Flash Professional CS5.5 or later. ProLoader is not needed in environments other than Flash Professional.

**_Important:_ **_For projects published in Flash Professional CS5.5 and later, you can always use the fl.display.ProLoader and fl.display.ProLoaderInfo classes instead of flash.display.Loader and flash.display.LoaderInfo._

Issues addressed by the ProLoader class

The ProLoader class addresses issues that the legacy Loader class was not designed to handle. These issues stem from RSL preloading of TLF libraries. Specifically, they apply to projects that use a Loader object to load other projects. Addressed issues include the following:

*   **Scripting between the loading file and the loaded file does not behave as expected.** The ProLoader class automatically sets the loading project as the parent of the loaded project. Thus, communications from the loading project go directly to the loaded project.
*   **The SWF application must actively manage the loading process.** Doing so requires implementation of extra events, such as added, removed, addedToStage, and removedFromStage. If your application targets OpenFL

10.2 or later, ProLoader removes the need for this extra work.

Updating code to use ProLoader instead of Loader

Because ProLoader mirrors the Loader class, you can easily switch the two classes in your code. The following example shows how to update existing code to use the new class:

import flash.display.Loader; import flash.events.Event; var l:Loader = new Loader();

addChild(l);

l.contentLoaderInfo.addEventListener(Event.COMPLETE, loadComplete); l.load(&quot;my.swf&quot;);

function loadComplete(e:Event) { trace(&#039;load complete!&#039;);

}

This code can be updated to use ProLoader as follows:

import **fl.display.ProLoader**; import flash.events.Event;

var l:**ProLoader** = new **ProLoader**();

addChild(l);

l.contentLoaderInfo.addEventListener(Event.COMPLETE, loadComplete); l.load(&quot;my.swf&quot;);

function loadComplete(e:Event) { trace(&#039;load complete!&#039;);

}
-->