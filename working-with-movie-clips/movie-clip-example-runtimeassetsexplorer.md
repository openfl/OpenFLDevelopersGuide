# Movie clip example: RuntimeAssetsExplorer {#movie-clip-example-runtimeassetsexplorer}

The Export for Haxe functionality can be especially advantageous for libraries that may be useful across more than one project. If OpenFL executes a project, symbols that have been exported to Haxe are available to any project within the same security sandbox as the SWF that loads it. In this way, a single Flash document can generate a project that is designated for the sole purpose of holding graphical assets. This technique is particularly useful for larger projects where designers working on visual assets can work in parallel with developers who create a "wrapper" project that then loads the graphical assets project at run time. You can use this method to maintain a series of versioned files where graphical assets are not dependent upon the progress of programming development.

The RuntimeAssetsExplorer application loads any project that is a subclass of RuntimeAsset and allows you to browse the available assets of that project. The example illustrates the following:

*   Loading an external project using Loader.load()
*   Dynamic creation of a library symbol exported for Haxe
*   Haxe control of MovieClip playback

Before beginning, note that each of the projects to run in OpenFL must be located in the same security sandbox. For more information, see

"Security sandboxes" on page 1044

.

To get the application files for this sample, download the [Flash Professional Samples](http://help.adobe.com/support/documentation/en/flash/10/Flash_Haxe3.0_samples_CS4.zip)[.](http://www.adobe.com/go/learn_programmingAS3samples_flash) The RuntimeAssetsExplorer application files can be found in the folder Samples/RuntimeAssetsExplorer. The application consists of the following files:

| **File** | **Description** |
| --- | --- |
| RuntimeAssetsExample.mxml or | The user interface for the application for Flex (MXML) or Flash (FLA). |
| RuntimeAssetsExample.as | Document class for the Flash (FLA) application. |
| GeometricAssets.as | An example class that implements the RuntimeAsset interface. |
| GeometricAssets.fla | A FLA file linked to the GeometricAssets class (the document class of the FLA) containing symbols that are exported for Haxe. |
| com/example/programmingas3/runtimeassetexplorer/RuntimeLibrary.as | An interface that defines the required methods expected of all run-time asset projects that will be loaded into the explorer container. |
| com/example/programmingas3/runtimeassetexplorer/AnimatingBox.as | The class of the library symbol in the shape of a rotating box. |
| com/example/programmingas3/runtimeassetexplorer/AnimatingStar.as | The class of the library symbol in the shape of a rotating star. |

**Establishing a run-time library interface**

In order for the explorer to properly interact with a SWF library, the structure of the run-time asset libraries must be formalized. We will accomplish this by creating an interface, which is similar to a class in that it’s a blueprint of methods that demarcate an expected structure, but unlike a class it includes no method bodies. The interface provides a way for both the run-time library and the explorer to communicate to one another. Each SWF of run-time assets that is loaded in our browser will implement this interface. For more information about interfaces and how they can be useful, see Interfaces in _Learning Haxe_.

The RuntimeLibrary interface will be very simple—we merely require a function that can provide the explorer with an array of classpaths for the symbols to be exported and available in the run-time library. To this end, the interface has a single method: getAssets().

package com.example.programmingas3.runtimeassetexplorer

{

public interface RuntimeLibrary

{

function getAssets():Array;

}

}

## Creating the asset library project {#creating-the-asset-library-swf-file}

By defining the RuntimeLibrary interface, it’s possible to create multiple asset library projects that can be loaded into another project. Making an individual SWF library of assets involves four tasks:

*   Creating a class for the asset library project
*   Creating classes for individual assets contained in the library
*   Creating the actual graphic assets
*   Associating graphic elements with classes and publishing the library SWF

Creating a class to implement the RuntimeLibrary interface

Next, we’ll create the GeometricAssets class that will implement the RuntimeLibrary interface. This will be the document class of the FLA. The code for this class is very similar to the RuntimeLibrary interface—the difference between them is that in the class definition the getAssets() method has a method body.

package

{

import openfl.display.Sprite;

import com.example.programmingas3.runtimeassetexplorer.RuntimeLibrary;

public class GeometricAssets extends Sprite implements RuntimeLibrary

{

public function GeometricAssets() {

}

public function getAssets():Array {

return [ "com.example.programmingas3.runtimeassetexplorer.AnimatingBox", "com.example.programmingas3.runtimeassetexplorer.AnimatingStar" ];

}

}

}

If we were to create a second run-time library, we could create another FLA based upon another class (for example, AnimationAssets) that provides its own getAssets() implementation.

Creating classes for each MovieClip asset

For this example, we’ll merely extend the MovieClip class without adding any functionality to the custom assets. The following code for AnimatingStar is analogous to that of AnimatingBox:

package com.example.programmingas3.runtimeassetexplorer

{

import openfl.display.MovieClip;

public class AnimatingStar extends MovieClip

{

public function AnimatingStar() {

}

}

}

Publishing the library

We’ll now connect the MovieClip-based assets to the new class by creating a new FLA and entering GeometricAssets into the Document Class field of the Property inspector. For the purposes of this example, we’ll create two very basic shapes that use a timeline tween to make one clockwise rotation over 360 frames. Both the animatingBox and animatingStar symbols are set to Export for Haxe and have the Class field set to the respective classpaths specified in the getAssets() implementation. The default base class of openfl.display.MovieClip remains, as we want to subclass the standard MovieClip methods.

After setting up your symbol’s export settings, publish the FLA. You now have your first run-time library. This project could be loaded into another AVM2 project and the AnimatingBox and AnimatingStar symbols would be available to the new project.

## Loading the library into another project {#loading-the-library-into-another-swf-file}

The last functional piece to deal with is the user interface for the asset explorer. In this example, the path to the run- time library is hard-coded as a variable named ASSETS_PATH. Alternatively, you could use the FileReference class—for example, to create an interface that browses for a particular project on your hard drive.

When the run-time library is successfully loaded, OpenFL calls the runtimeAssetsLoadComplete() method:

private function runtimeAssetsLoadComplete(event:Event):void

{

var rl:* = event.target.content;

var assetList:Array = rl.getAssets(); populateDropdown(assetList); stage.frameRate = 60;

}

In this method, the variable rl represents the loaded project. The code calls the getAssets() method of the loaded project, obtaining the list of assets that are available, and uses them to populate a ComboBox component with a list of available assets by calling the populateDropDown() method. That method in turn stores the full classpath of each asset. Clicking the Add button on the user interface triggers the addAsset() method:

private function addAsset():void

{

var className:String = assetNameCbo.selectedItem.data;

var AssetClass:Class = getDefinitionByName(className) as Class; var mc:MovieClip = new AssetClass();

...

}

which gets the classpath of whichever asset is currently selected in the ComboBox (assetNameCbo.selectedItem.data), and uses the getDefinitionByName() function (from the openfl.utils package) to obtain an actual reference to the asset’s class in order to create a new instance of that asset.