# Caching display objects {#caching-display-objects}

As your designs in OpenFL grow in size, whether you are creating an application or complex scripted animations, you need to consider performance and optimization. When you have content that remains static (such as a rectangle Shape instance), OpenFL does not optimize the content. Therefore, when you change the position of the rectangle, OpenFL redraws the entire Shape instance.

You can cache specified display objects to improve the performance of your project. The display object is a _surface_, essentially a bitmap version of the instance’s vector data, which is data that you do not intend to change much over the course of your project. Therefore, instances with caching turned on are not continually redrawn as the project plays, letting the project render quickly.

**_Note:_** _You can update the vector data, at which time the surface is recreated. Therefore, the vector data cached in the surface does not need to remain the same for the entire project._

Setting a display object’s `cacheAsBitmap` property to `true` makes the display object cache a bitmap representation of itself. OpenFL creates a surface object for the instance, which is a cached bitmap instead of vector data. If you change the bounds of the display object, the surface is recreated instead of resized. Surfaces can nest within other surfaces. The child surface copies its bitmap onto its parent surface. For more information, see [Enabling bitmap caching](#enabling-bitmap-caching).

The DisplayObject class’s `opaqueBackground` property and `scrollRect` property are related to bitmap caching using the `cacheAsBitmap` property. Although these three properties are independent of each other, the `opaqueBackground` and `scrollRect` properties work best when an object is cached as a bitmap—you see performance benefits for the `opaqueBackground` and `scrollRect` properties only when you set `cacheAsBitmap` to `true`. For more information about scrolling display object content, see [Panning and scrolling display objects](panning-and-scrolling-display-objects.md). For more information about setting an opaque background, see [Setting an opaque background color](setting-an-opaque-background.md).

For information on alpha channel masking, which requires you to set the `cacheAsBitmap` property to `true`, see [Masking display objects](masking-display-objects.md).

## When to enable caching {#when-to-enable-caching}

Enabling caching for a display object creates a surface, which has several advantages, such as helping complex vector animations to render fast. There are several scenarios in which you will want to enable caching. It might seem as though you would always want to enable caching to improve the performance of your projects; however, there are situations in which enabling caching does not improve performance, or can even decrease it. This section describes scenarios in which caching should be used, and when to use regular display objects.

Overall performance of cached data depends on how complex the vector data of your instances are, how much of the data you change, and whether or not you set the `opaqueBackground` property. If you are changing small regions, the difference between using a surface and using vector data could be negligible. You might want to test both scenarios with your work before you deploy the application.

### When to use bitmap caching

The following are typical scenarios in which you might see significant benefits when you enable bitmap caching.

*   Complex background image: An application that contains a detailed and complex background image of vector data (perhaps an image where you applied the trace bitmap command, or artwork that you created in Adobe Illustrator®). You might animate characters over the background, which slows the animation because the background needs to continuously regenerate the vector data. To improve performance, you can set the `opaqueBackground` property of the background display object to `true`. The background is rendered as a bitmap and can be redrawn quickly, so that your animation plays much faster.
*   Scrolling text field: An application that displays a large amount of text in a scrolling text field. You can place the text field in a display object that you set as scrollable with scrolling bounds (the `scrollRect` property). This enables fast pixel scrolling for the specified instance. When a user scrolls the display object instance, OpenFL shifts the scrolled pixels up and generates the newly exposed region instead of regenerating the entire text field.
*   Windowing system: An application with a complex system of overlapping windows. Each window can be open or closed (for example, web browser windows). If you mark each window as a surface (by setting the `cacheAsBitmap` property to `true`), each window is isolated and cached. Users can drag the windows so that they overlap each other, and each window doesn’t need to regenerate the vector content.
*   Alpha channel masking: When you are using alpha channel masking, you must set the `cacheAsBitmap` property to `true`. For more information, see [Masking display objects](masking-display-objects.md).

Enabling bitmap caching in all of these scenarios improves the responsiveness and interactivity of the application by optimizing the vector graphics.

In addition, whenever you apply a filter to a display object, `cacheAsBitmap` is automatically set to `true`, even if you explicitly set it to `false`. If you clear all the filters from the display object, the `cacheAsBitmap` property returns to the value it was last set to.

### When to avoid using bitmap caching

Using this feature in the wrong circumstances can negatively affect the performance of your project. When you use bitmap caching, remember the following guidelines:

*   Do not overuse surfaces (display objects with caching enabled). Each surface uses more memory than a regular display object, which means that you should only enable surfaces when you need to improve rendering performance.

A cached bitmap can use significantly more memory than a regular display object. For example, if a Sprite instance on the Stage is 250 pixels by 250 pixels in size, when cached it might use 250 KB instead of 1 KB when it’s a regular (un-cached) Sprite instance.

*   Avoid zooming into cached surfaces. If you overuse bitmap caching, a large amount of memory is consumed (see previous bullet), especially if you zoom in on the content.
*   Use surfaces for display object instances that are largely static (non-animating). You can drag or move the instance, but the contents of the instance should not animate or change a lot. (Animation or changing content are more likely with a MovieClip instance containing animation or a Video instance.) For example, if you rotate or transform an instance, the instance changes between the surface and vector data, which is difficult to process and negatively affects your project.
*   If you mix surfaces with vector data, it increases the amount of processing that OpenFL (and sometimes the computer) need to do. Group surfaces together as much as possible—for example, when you create windowing applications.
*   Do not cache objects whose graphics change frequently. Every time you scale, skew, rotate the display object, change the alpha or color transform, move child display objects, or draw using the graphics property, the bitmap cache is redrawn. If this happens every frame, the runtime must draw the object into a bitmap and then copy that bitmap onto the stage—which results in extra work compared to just drawing the uncached object to the stage. The performance tradeoff of caching versus update frequency depends on the complexity and size of the display object and can only be determined by testing the specific content.

## Enabling bitmap caching {#enabling-bitmap-caching}

To enable bitmap caching for a display object, you set its `cacheAsBitmap` property to `true`: `mySprite.cacheAsBitmap = true;`

After you set the `cacheAsBitmap` property to `true`, you might notice that the display object automatically pixel-snaps to whole coordinates. When you test the project, you should notice that any animation performed on a complex vector image renders much faster.

A surface (cached bitmap) is not created, even if `cacheAsBitmap` is set to true, if one or more of the following occurs:

*   The bitmap is greater than 2880 pixels in height or width.
*   The bitmap fails to allocate (because of an out-of-memory error).

<!--
### Cached bitmap transform matrices

**Adobe AIR 2.0 and later (mobile profile)**

In AIR applications for mobile devices, you should set the `cacheAsBitmapMatrix` property whenever you set the cacheAsBitmap property. Setting this property allows you to apply a wider range of transformations to the display object without triggering rerendering.

```haxe
mySprite.cacheAsBitmap = true;
mySprite.cacheAsBitmapMatrix = new Matrix ();
```

When you set this matrix property, you can apply the following additional transformation to the display object without recaching the object:

*   Move or translate without pixel-snapping
*   Rotate
*   Scale
*   Skew
*   Change alpha (between 0 and 100% transparency)

These transformations are applied directly to the cached bitmap.
-->

## Setting an opaque background color {#setting-an-opaque-background-color}

You can set an opaque background for a display object. For example, when your project has a background that contains complex vector art, you can set the `opaqueBackground` property to a specified color (typically the same color as the Stage). The color is specified as a number (commonly a hexadecimal color value). The background is then treated as a bitmap, which helps optimize performance.

When you set `cacheAsBitmap` to `true`, and also set the `opaqueBackground` property to a specified color, the `opaqueBackground` property allows the internal bitmap to be opaque and rendered faster. If you do not set `cacheAsBitmap` to `true`, the `opaqueBackground` property adds an opaque vector-square shape to the background of the display object. It does not create a bitmap automatically.

The following example shows how to set the background of a display object to optimize performance:

```haxe
myShape.cacheAsBitmap = true;
myShape.opaqueBackground = 0xFF0000;
```

In this case, the background color of the Shape named `myShape` is set to red (`0xFF0000`). Assuming the Shape instance contains a drawing of a green triangle, on a Stage with a white background, this would show up as a green triangle with red in the empty space in the Shape instance’s bounding box (the rectangle that completely encloses the Shape).

![](/assets/dp_opaqueBackground_example.png)

Of course, this code would make more sense if it were used with a Stage with a solid red background. On another colored background, that color would be specified instead. For example, in a project with a white background, the `opaqueBackground` property would most likely be set to `0xFFFFFF`, or pure white.
