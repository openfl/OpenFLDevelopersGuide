# Working with MovieClip objects {#working-with-movieclip-objects}

When you publish a project, Flash converts all movie clip symbol instances on the Stage to MovieClip objects. You can make a movie clip symbol available to your Haxe code by giving it an instance name in the Instance Name field of the Property inspector. When the project is created, Flash generates the code that creates the MovieClip instance on the Stage and declares a variable using the instance name. If you have named movie clips that are nested inside other named movie clips, those child movie clips are treated like properties of the parent movie clip—you can access the child movie clip using dot syntax. For example, if a movie clip with the instance name `childClip` is nested within another clip with the instance name `parentClip`, you can make the child clip’s timeline animation play by calling this code:

```haxe
parentClip.childClip.play ();
```

**_Note:_** _: Children instances placed on the Stage in the Flash authoring tool cannot be accessed by code from within the constructor of a parent instance since they have not been created at that point in code execution. Before accessing the child, the parent must instead either create the child instance by code or delay access to a callback function that listens for the child to dispatch its `Event.ADDED_TO_STAGE` event._

While some legacy methods and properties of the ActionScript 2.0 MovieClip class remain the same, others have changed. All properties prefixed with an underscore have been renamed. For example, _width and _height properties are now accessed as width and height, while _xscale and _yscale are now accessed as scaleX and scaleY. For a complete list of the properties and methods of the MovieClip class, consult the [API Reference](http://api.openfl.org/openfl/display/MovieClip.html).