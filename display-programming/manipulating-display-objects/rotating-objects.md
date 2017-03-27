# Rotating objects {#rotating-objects}

Display objects can be rotated using the `rotation` property. You can read this value to find out whether an object has been rotated, or to rotate the object you can set this property to a number (in degrees) representing the amount of rotation to be applied to the object. For instance, this line of code rotates the object named `square` 45 degrees (one eighth of one complete revolution):

```haxe
square.rotation = 45;
```

Alternatively, you can rotate a display object using a transformation matrix, described in [Working with geometry](/working-with-geometry/README.md).