# Applying blending modes {#applying-blending-modes}

Blending modes involve combining the colors of one image (the base image) with the colors of another image (the blend image) to produce a third image—the resulting image is the one that is actually displayed on the screen. Each pixel value in an image is processed with the corresponding pixel value of the other image to produce a pixel value for that same position in the result.

Every display object has a `blendMode` property that can be set to one of the following blending modes. These are constants defined in the BlendMode class. Alternatively, you can use the String values (in parentheses) that are the actual values of the constants.

*   `BlendMode.ADD` (&quot;add&quot;): Commonly used to create an animated lightening dissolve effect between two images.
*   `BlendMode.ALPHA` (&quot;alpha&quot;): Commonly used to apply the transparency of the foreground on the background. (Not supported under GPU rendering.)
*   `BlendMode.DARKEN` (&quot;darken&quot;): Commonly used to superimpose type. (Not supported under GPU rendering.)
*   `BlendMode.DIFFERENCE` (&quot;difference&quot;): Commonly used to create more vibrant colors.
*   `BlendMode.ERASE` (&quot;erase&quot;): Commonly used to cut out (erase) part of the background using the foreground alpha. (Not supported under GPU rendering.)
*   `BlendMode.HARDLIGHT` (&quot;hardlight&quot;): Commonly used to create shading effects. (Not supported under GPU rendering.)
*   `BlendMode.INVERT` (&quot;invert&quot;): Used to invert the background.
*   `BlendMode.LAYER` (&quot;layer&quot;): Used to force the creation of a temporary buffer for precomposition for a particular display object. (Not supported under GPU rendering.)
*   `BlendMode.LIGHTEN` (&quot;lighten&quot;): Commonly used to superimpose type. (Not supported under GPU rendering.)
*   `BlendMode.MULTIPLY` (&quot;multiply&quot;): Commonly used to create shadows and depth effects.
*   `BlendMode.NORMAL` (&quot;normal&quot;): Used to specify that the pixel values of the blend image override those of the base image.
*   `BlendMode.OVERLAY` (&quot;overlay&quot;): Commonly used to create shading effects. (Not supported under GPU rendering.)
*   `BlendMode.SCREEN` (&quot;screen&quot;): Commonly used to create highlights and lens flares.
*   `BlendMode.SUBTRACT` (&quot;subtract&quot;): Commonly used to create an animated darkening dissolve effect between two images.

<!--
*   `BlendMode.SHADER` (&quot;shader&quot;): Used to specify that a Pixel Bender shader is used to create a custom blending effect. For more information about using shaders, see

    “Working with Pixel Bender shaders” on page 300

    . (Not supported under GPU rendering.)
-->

