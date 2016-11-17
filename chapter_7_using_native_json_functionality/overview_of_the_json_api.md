## Overview of the JSON API {#overview-of-the-json-api}

The ActionScript JSON API consists of the JSON class and toJSON() member functions on a few native classes. For applications that require a custom JSON encoding for any class, the ActionScript framework provides ways to override the default encoding.

The JSON class internally handles import and export for any ActionScript class that does not provide a toJSON() member. For such cases, JSON traverses the public properties of each object it encounters. If an object contains other objects, JSON recurses into the nested objects and performs the same traversal. If any object provides a toJSON() method, JSON uses that custom method instead of its internal algorithm.

The JSON interface consists of an encoding method, stringify(), and a decoding method, parse(). Each of these methods provides a parameter that lets you insert your own logic into the JSON encoding or decoding workflow. For stringify(), this parameter is named replacer; for parse(), it is reviver. These parameters take a function definition with two arguments using the following signature:

function(k, v):*

### toJSON() methods {#tojson-methods}

The signature for toJSON() methods is

public function toJSON(k:String):*

JSON.stringify() calls toJSON(), if it exists, for each public property that it encounters during its traversal of an object. A property consists of a key-value pair. When stringify() calls toJSON(), it passes in the key, k, of the property that it is currently examining. A typical toJSON() implementation evaluates each property name and returns the desired encoding of its value.

The toJSON() method can return a value of any type (denoted as *)—not just a String. This variable return type allows toJSON() to return an object if appropriate. For example, if a property of your custom class contains an object from another third-party library, you can return that object when toJSON() encounters your property. JSON then recurses into the third-party object. The encoding process flow behaves as follows:

*   If toJSON() returns an object that doesn’t evaluate to a string, stringify() recurses into that object.
*   If toJSON() returns a string, stringify() wraps that value in another string, returns the wrapped string, and then moves to the next value.

In many cases, returning an object is preferable to returning a JSON string created by your application. Returning an object takes leverages the built-in JSON encoding algorithm and also allows JSON to recurse into nested objects.

The toJSON()method is not defined in the Object class or in most other native classes. Its absence tells JSON to perform its standard traversal over the object&#039;s public properties. If you like, you can also use toJSON() to expose your object’s private properties.

A few native classes pose challenges that the ActionScript libraries can&#039;t solve effectively for all use cases. For these classes, ActionScript provides a trivial implementation that clients can reimplement to suit their needs. The classes that provide trivial toJSON() members include:

*   ByteArray
*   Date
*   Dictionary
*   XML

You can subclass the ByteArray class to override its toJSON() method, or you can redefine its prototype. The Date and XML classes, which are declared final, require you to use the class prototype to redefine toJSON(). The Dictionary class is declared dynamic, which gives you extra freedom in overriding toJSON().