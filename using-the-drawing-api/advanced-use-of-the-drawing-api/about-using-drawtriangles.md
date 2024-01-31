# About using drawTriangles() {#about-using-drawtriangles}

Another advanced method in OpenFL, [Graphics.drawTriangles()](http://api.openfl.org/openfl/display/Graphics.html#drawTriangles), is like the [Graphics.drawPath()](http://api.openfl.org/openfl/display/Graphics.html#drawPath) method. The `Graphics.drawTriangles()` method also uses a Vector<Float> object to specify point locations for drawing a path.

However, the real purpose for the `Graphics.drawTriangles()` method is to facilitate three-dimensional effects through OpenFL.

<!-- TODO: uncomment when this content is adapted for OpenFL
For information about using `Graphics.drawTriangles()` to produce three-dimensional effects, see [Using triangles for 3D effects](/using-triangles-for-3d-effects/README.md).-->

> OpenFL supports 3D using Stage3D, as well as the OpenGLView object. These are recommended over `drawTriangles()` for 3D