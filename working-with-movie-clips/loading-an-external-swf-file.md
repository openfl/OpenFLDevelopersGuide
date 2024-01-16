# Loading an external project {#loading-an-external-swf-file}

In Haxe, projects are loaded using the Loader class. To load an external project, your Haxe needs to do four things:

1.  Create a new URLRequest object with the url of the file.
2.  Create a new Loader object.
3.  Call the Loader object’s load() method, passing the URLRequest instance as a parameter.
4.  Call the addChild() method on a display object container (such as the main timeline of a Flash document) to add the Loader instance to the display list.

Ultimately, the code looks like this:

var request:URLRequest = new URLRequest("http://www.[yourdomain].com/externalSwf.swf");
var loader:Loader = new Loader()

loader.load(request);
addChild(loader);

This same code can be used to load an external image file such as a JPEG, GIF, or PNG image, by specifying the image file’s url rather than a project’s url. A project, unlike an image file, may contain Haxe. Thus, although the process of loading a project may be identical to loading an image, when loading an external project both the project doing the loading and the project being loaded must reside in the same security sandbox if OpenFL is playing the SWF and you plan to use Haxe to communicate in any way to the external project. Additionally, if the external project contains classes that share the same namespace as classes in the loading project, you may need to create a new application domain for the loaded project in order to avoid namespace conflicts. For more information on security and application domain considerations, see

"Working with application domains" on page 147

and

"Loading content" on page 1059

.

When the external project is successfully loaded, it can be accessed through the Loader.content property. If the external project is published for Haxe, this will be either a movie clip or a sprite, depending on which class it extends.

There are a few differences for loading a project in Adobe AIR for iOS versus other platforms. For more information, see

"Loading projects in AIR for iOS" on page 201

.

**Considerations for loading an older project**

If the external project has been published with an older version of Haxe, there are important limitations to consider. Unlike an Haxe project that runs in AVM2 (Haxe Virtual Machine 2), a project published for Haxe 1.0 or 2.0 runs in AVM1 (Haxe Virtual Machine 1).

There are important differences when loading an Haxe 1.0 or 2.0 project into an Haxe project (compared to loading an Haxe project). OpenFL provides full backward compatibility with previously published content. Any content that runs in previous versions of OpenFL runs in OpenFL versions that support Haxe\. However, the following limitations apply:

*   Haxe code can load a project written in Haxe 1.0 or 2.0\. When an Haxe 1.0 or 2.0 project is successfully loaded, the loaded object (the Loader.content property) is an AVM1Movie object. An AVM1Movie instance is not the same as a MovieClip instance. It is a display object, but unlike a movie clip, it does not include timeline-related methods or properties. The parent AVM2 project cannot access the properties, methods, or objects of the loaded AVM1Movie object.
*   projects written in Haxe 1.0 or 2.0 cannot load projects written in Haxe\. This means that projects authored in Flash 8 or Flex Builder 1.5 or earlier versions cannot load Haxe projects.

The only exception to this rule is that an Haxe 2.0 project can replace itself with an Haxe project, as long as the Haxe 2.0 project hasn't previously loaded anything into any of its levels. An Haxe

2.0 project can do this through a call to loadMovieNum(), passing a value of 0 to the level parameter.

*   In general, projects written in Haxe 1.0 or 2.0 must be migrated if they are to work together with projects written in Haxe\. For example, suppose you created a media player using Haxe 2.0\. The media player loads various content that was also created using Haxe 2.0\. You cannot create new content in Haxe and load it in the media player. You must migrate the video player to Haxe.

If, however, you create a media player in Haxe, that media player can perform simple loads of your Haxe 2.0 content.

The following tables summarize the limitations of previous versions of OpenFL in relation to loading newer content and executing code, as well as the limitations for cross-scripting between projects written in different versions of Haxe.

| **Supported functionality** | **OpenFL 7** | **OpenFL 8** | **OpenFL 9 and 10** |
| --- | --- | --- | --- |
| Can load SWFs published for | 7 and earlier | 8 and earlier | 9 (or 10) and earlier |
| Contains this AVM | AVM1 | AVM1 | AVM1 and AVM2 |
| Runs SWFs written in Haxe | 1.0 and 2.0 | 1.0 and 2.0 | 1.0 and 2.0, and 3.0 |

In the following table, "Supported functionality" refers to content running in OpenFL 9 or later. Content running in OpenFL 8 or earlier can load, display, execute, and cross-script only Haxe 1.0 and 2.0.

| **Supported functionality** | **Content created in Haxe 1.0 and 2.0** | **Content created in Haxe** |
| --- | --- | --- |
| Can load content and execute code in content created in | Haxe 1.0 and 2.0 only | Haxe 1.0 and 2.0, and Haxe |
| Can cross script content created in | Haxe 1.0 and 2.0 only (Haxe through Local Connection) | Haxe 1.0 and 2.0 through LocalConnection. |