## The Graphics class {#the-graphics-class}

Flash Player 9 and later, Adobe AIR 1.0 and later

Each Shape, Sprite, and MovieClip object has a graphics property, which is an instance of the Graphics class. The Graphics class includes properties and methods for drawing lines, fills, and shapes. If you want a display object to use solely as a canvas for drawing content, you can use a Shape instance. A Shape instance will perform better than other display objects for drawing, because it doesn’t have the overhead of the additional functionality in the Sprite and MovieClip classes. If you want a display object on which you can draw graphical content and also want that object to contain other display objects, you can use a Sprite instance. For more information on determining which display object to use for various tasks, see

“Choosing a DisplayObject subclass” on page 172

.