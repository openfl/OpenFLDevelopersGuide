# Traversing the display list {#traversing-the-display-list}

As you’ve seen, the display list is a tree structure. At the top of the tree is the Stage, which can contain multiple display objects. Those display objects that are themselves display object containers can contain other display objects, or display object containers.

![](/assets/dp_Display_List_Organization.png)

The DisplayObjectContainer class includes properties and methods for traversing the display list, by means of the child lists of display object containers. For example, consider the following code, which adds two display objects, title and pict, to the container object (which is a Sprite, and the Sprite class extends the DisplayObjectContainer class):

```haxe
var container = new Sprite();

var title = new TextField ();
title.text = "Hello";

var pict = new Loader ();
var url = new URLRequest ("banana.jpg");
pict.load (url);
pict.name = "banana loader";

container.addChild (title);
container.addChild (pict);
```

The `getChildAt()` method returns the child of the display list at a specific index position:

```haxe
trace (Std.is (container.getChildAt (0), TextField)); // true
```

You can also access child objects by name. Each display object has a name property, and if you don’t assign it, OpenFL assigns a default value, such as "instance1". For example, the following code shows how to use the `getChildByName()` method to access a child display object with the name "banana loader":

```haxe
trace (Std.is (container.getChildByName ("banana"), Loader)); // true
```

Using the `getChildByName()` method can result in slower performance than using the `getChildAt()` method.

Since a display object container can contain other display object containers as child objects in its display list, you can traverse the full display list of the application as a tree. For example, in the code excerpt shown earlier, once the load operation for the `pict` Loader object is complete, the `pict` object will have one child display object, which is the bitmap, loaded. To access this bitmap display object, you can write `pict.getChildAt (0)`. You can also write `container.getChildAt (0).getChildAt (0)` (since `container.getChildAt (0) == pict`).

The following function provides an indented `trace()` output of the display list from a display object container:

```haxe
public function traceDisplayList (container:DisplayObjectContainer, indentString:String = ""):Void {
	
	var child:DisplayObject;
	
	for (i in 0...container.numChildren) {
		
		child = container.getChildAt (i);
		trace (indentString + child + child.name);
		
		if (Std.is (container.getChildAt (i), DisplayObjectContainer)) {
			
			traceDisplayList (cast (child, DisplayObjectContainer), indentString + "	");
			
		}
		
	}
	
}
```