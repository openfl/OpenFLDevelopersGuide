## Defining custom JSON behavior {#defining-custom-json-behavior}

To implement your own JSON encoding and decoding for native classes, you can choose from several options:

*   Defining or overriding toJSON()on your custom subclass of a non-final native class
*   Defining or redefining toJSON() on the class prototype
*   Defining a toJSON property on a dynamic class
*   Using the JSON.stringify() replacer and JSON.parser() reviver parameters

More Help topics

[ECMA-262, 5th Edition](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf)

**Defining toJSON() on the prototype of a built-in class**

The native JSON implementation in ActionScript mirrors the ECMAScript JSON mechanism defined in ECMA-262, 5th edition. Since ECMAScript doesn&#039;t support classes, ActionScript defines JSON behavior in terms of prototype- based dispatch. Prototypes are precursors to ActionScript 3.0 classes that allow simulated inheritance as well as member additions and redefinitions.

ActionScript allows you to define or redefine toJSON() on the prototype of any class. This privilege applies even to classes that are marked final. When you define toJSON() on a class prototype, your definition becomes current for all instances of that class within the scope of your application. For example, here&#039;s how you can define a toJSON() method on the MovieClip prototype:

MovieClip.prototype.toJSON = function(k):* { trace(&quot;prototype.toJSON() called.&quot;); return &quot;toJSON&quot;;

}

When your application then calls stringify() on any MovieClip instance, stringify() returns the output of your

toJSON() method:

var mc:MovieClip = new MovieClip();

var js:String = JSON.stringify(mc); //&quot;prototype toJSON() called.&quot; trace(&quot;js: &quot; + js); //&quot;js: toJSON&quot;

You can also override toJSON() in native classes that define the method. For example, the following code overrides

Date.toJSON():

Date.prototype.toJSON = function (k):* {

return &quot;any date format you like via toJSON: &quot;+ &quot;this.time:&quot;+this.time + &quot; this.hours:&quot;+this.hours;

}

var dt:Date = new Date(); trace(JSON.stringify(dt));

// &quot;any date format you like via toJSON: this.time:1317244361947 this.hours:14&quot;

### Defining or overriding toJSON() at the class level {#defining-or-overriding-tojson-at-the-class-level}

Applications aren&#039;t always required to use prototypes to redefine toJSON(). You can also define toJSON() as a member of a subclass if the parent class is not marked final. For example, you can extend the ByteArray class and define a public toJSON() function:

package {

import flash.utils.ByteArray;

public class MyByteArray extends ByteArray

{

public function MyByteArray() {

}

public function toJSON(s:String):*

{

return &quot;MyByteArray&quot;;

}

}

}

var ba:ByteArray = new ByteArray(); trace(JSON.stringify(ba)); //&quot;ByteArray&quot;

var mba:MyByteArray = new MyByteArray(); //&quot;MyByteArray&quot; trace(JSON.stringify(mba)); //&quot;MyByteArray&quot;

If a class is dynamic, you can add a toJSON property to an object of that class and assign a function to it as follows:

var d:Dictionary = new Dictionary(); trace(JSON.stringify((d))); // &quot;Dictionary&quot;

d.toJSON = function(){return {c : &quot;toJSON override.&quot;};} // overrides existing function trace(JSON.stringify((d))); // {&quot;c&quot;:&quot;toJSON override.&quot;}

You can override, define, or redefine toJSON() on any ActionScript class. However, most built-in ActionScript classes don&#039;t define toJSON(). The Object class does not define toJSON in its default prototype or declare it as a class member. Only a handful of native classes define the method as a prototype function. Thus, in most classes you can’t override toJSON() in the traditional sense.

Native classes that don’t define toJSON() are serialized to JSON by the internal JSON implementation. Avoid replacing this built-in functionality if possible. If you define a toJSON() member, the JSON class uses your logic instead of its own functionality.

### Using the JSON.stringify() replacer parameter {#using-the-json-stringify-replacer-parameter}

Overriding toJSON() on the prototype is useful for changing a class’s JSON export behavior throughout an application. In some cases, though, your export logic might apply only to special cases under transient conditions. To accommodate such small-scope changes, you can use the replacer parameter of the JSON.stringify() method.

The stringify() method applies the function passed through the replacer parameter to the object being encoded. The signature for this function is similar to that of toJSON():

function (k,v):*

Unlike toJSON(), the replacer function requires the value, v, as well as the key, k. This difference is necessary because stringify() is defined on the static JSON object instead of the object being encoded. When JSON.stringify() calls replacer(k,v), it is traversing the original input object. The implicit this parameter passed to the replacer function refers to the object that holds the key and value. Because JSON.stringify() does not modify the original input object, that object remains unchanged in the container being traversed. Thus, you can use the code this[k]to query the key on the original object. The v parameter holds the value that toJSON() converts.

Like toJSON(), the replacer function can return any type of value. If replacer returns a string, the JSON engine escapes the contents in quotes and then wraps those escaped contents in quotes as well. This wrapping guarantees that stringify() receives a valid JSON string object that remains a string in a subsequent call to JSON.parse().

The following code uses the replacer parameter and the implicit this parameter to return the time and hours values of a Date object:

JSON.stringify(d, function (k,v):* {

return &quot;any date format you like via replacer: &quot;+ &quot;holder[k].time:&quot;+this[k].time + &quot; holder[k].hours:&quot;+this[k].hours;

});

### Using the JSON.parse() reviver parameter {#using-the-json-parse-reviver-parameter}

The reviver parameter of the JSON.parse() method does the opposite of the replacer function: It converts a JSON string into a usable ActionScript object. The reviver argument is a function that takes two parameters and returns any type:

function (k,v):*

In this function, k is a key, and v is the value of k. Like stringify(), parse() traverses the JSON key-value pairs and applies the reviver function—if one exists—to each pair. A potential problem is the fact that the JSON class does not output an object’s ActionScript class name. Thus, it can be challenging to know which type of object to revive. This problem can be especially troublesome when objects are nested. In designing toJSON(), replacer, and reviver functions, you can devise ways to identify the ActionScript objects that are exported while keeping the original objects intact.

### Parsing example {#parsing-example}

The following example shows a strategy for reviving objects parsed from JSON strings. This example defines two classes: JSONGenericDictExample and JSONDictionaryExtnExample. Class JSONGenericDictExample is a custom dictionary class. Each record contains a person’s name and birthday, as well as a unique ID. Each time the JSONGenericDictExample constructor is called, it adds the newly created object to an internal static array with a statically incrementing integer as its ID. Class JSONGenericDictExample also defines a revive() method that extracts just the integer portion from the longer id member. The revive() method uses this integer to look up and return the correct revivable object.

Class JSONDictionaryExtnExample extends the ActionScript Dictionary class. Its records have no set structure and can contain any data. Data is assigned after a JSONDictionaryExtnExample object is constructed, rather than as class- defined properties. JSONDictionaryExtnExample records use JSONGenericDictExample objects as keys. When a JSONDictionaryExtnExample object is revived, the JSONGenericDictExample.revive() function uses the ID associated with JSONDictionaryExtnExample to retrieve the correct key object.

Most importantly, the JSONDictionaryExtnExample.toJSON() method returns a marker string in addition to the JSONDictionaryExtnExample object. This string identifies the JSON output as belonging to the JSONDictionaryExtnExample class. This marker leaves no doubt as to which object type is being processed during JSON.parse().

package {

// Generic dictionary example:

public class JSONGenericDictExample { static var revivableObjects = []; static var nextId = 10000;

public var id;

public var dname:String; public var birthday;

public function JSONGenericDictExample(name, birthday) { revivableObjects[nextId] = this;

this.id = &quot;id_class_JSONGenericDictExample_&quot; + nextId; this.dname = name;

this.birthday = birthday; nextId++;

}

public function toString():String { return this.dname; }

public static function revive(id:String):JSONGenericDictExample { var r:RegExp = /^id_class_JSONGenericDictExample_([0-9]*)$/; var res = r.exec(id);

return JSONGenericDictExample.revivableObjects[res[1]];

}

}

}

package {

import flash.utils.Dictionary; import flash.utils.ByteArray;

// For this extension of dictionary, we serialize the contents of the

// dictionary by using toJSON

public final class JSONDictionaryExtnExample extends Dictionary { public function toJSON(k):* {

var contents = {}; for (var a in this) {

contents[a.id] = this[a];

}

// We also wrap the contents in an object so that we can

// identify it by looking for the marking property &quot;class E&quot;

// while in the midst of JSON.parse.

return {&quot;class JSONDictionaryExtnExample&quot;: contents};

}

// This is just here for debugging and for illustration public function toString():String {

var retval = &quot;[JSONDictionaryExtnExample &lt;&quot;; var printed_any = false;

for (var k in this) {

retval += k.toString() + &quot;=&quot; + &quot;[e=&quot;+this[k].earnings + &quot;,v=&quot;+this[k].violations + &quot;], &quot; printed_any = true;

}

if (printed_any)

retval = retval.substring(0, retval.length-2); retval += &quot;&gt;]&quot;

return retval;

}

}

}

When the following runtime script calls JSON.parse() on a JSONDictionaryExtnExample object, the reviver function calls JSONGenericDictExample.revive() on each object in JSONDictionaryExtnExample. This call extracts the ID that represents the object key. The JSONGenericDictExample.revive()function uses this ID to retrieve and return the stored JSONDictionaryExtnExample object from a private static array.

import flash.display.MovieClip; import flash.text.TextField;

var a_bob1:JSONGenericDictExample = new JSONGenericDictExample(&quot;Bob&quot;, new Date(Date.parse(&quot;01/02/1934&quot;)));

var a_bob2:JSONGenericDictExample = new JSONGenericDictExample(&quot;Bob&quot;, new Date(Date.parse(&quot;05/06/1978&quot;)));

var a_jen:JSONGenericDictExample = new JSONGenericDictExample(&quot;Jen&quot;, new Date(Date.parse(&quot;09/09/1999&quot;)));

var e = new JSONDictionaryExtnExample(); e[a_bob1] = {earnings: 40, violations: 2};

e[a_bob2] = {earnings: 10, violations: 1};

e[a_jen] = {earnings: 25, violations: 3};

trace(&quot;JSON.stringify(e): &quot; + JSON.stringify(e)); // {&quot;class JSONDictionaryExtnExample&quot;:

//{&quot;id_class_JSONGenericDictExample_10001&quot;:

//{&quot;earnings&quot;:10,&quot;violations&quot;:1},

//&quot;id_class_JSONGenericDictExample_10002&quot;:

//{&quot;earnings&quot;:25,&quot;violations&quot;:3},

//&quot;id_class_JSONGenericDictExample_10000&quot;:

// {&quot;earnings&quot;:40,&quot;violations&quot;:2}}} var e_result = JSON.stringify(e);

var e1 = new JSONDictionaryExtnExample(); var e2 = new JSONDictionaryExtnExample();

// It&#039;s somewhat easy to convert the string from JSON.stringify(e) back

// into a dictionary (turn it into an object via JSON.parse, then loop

// over that object&#039;s properties to construct a fresh dictionary).

//

// The harder exercise is to handle situations where the dictionaries

// themselves are nested in the object passed to JSON.stringify and

// thus does not occur at the topmost level of the resulting string.

//

// (For example: consider roundtripping something like

// var tricky_array = [e1, [[4, e2, 6]], {table:e3}]

// where e1, e2, e3 are all dictionaries. Furthermore, consider

// dictionaries that contain references to dictionaries.)

//

// This parsing (or at least some instances of it) can be done via

// JSON.parse, but it&#039;s not necessarily trivial. Careful consideration

// of how toJSON, replacer, and reviver can work together is

// necessary.

var e_roundtrip = JSON.parse(e_result,

// This is a reviver that is focused on rebuilding JSONDictionaryExtnExample objects. function (k, v) {

if (&quot;class JSONDictionaryExtnExample&quot; in v) { // special marker tag;

//see JSONDictionaryExtnExample.toJSON(). var e = new JSONDictionaryExtnExample();

var contents = v[&quot;class JSONDictionaryExtnExample&quot;]; for (var i in contents) {

// Reviving JSONGenericDictExample objects from string

// identifiers is also special;

// see JSONGenericDictExample constructor and

// JSONGenericDictExample&#039;s revive() method. e[JSONGenericDictExample.revive(i)] = contents[i];

}

return e;

} else {

return v;

}

});

trace(&quot;// == Here is an extended Dictionary that has been round-tripped ==&quot;); trace(&quot;// == Note that we have revived Jen/Jan during the roundtrip. ==&quot;);

trace(&quot;e: &quot; + e); //[JSONDictionaryExtnExample &lt;Bob=[e=40,v=2], Bob=[e=10,v=1],

//Jen=[e=25,v=3]&gt;]

trace(&quot;e_roundtrip: &quot; + e_roundtrip); //[JSONDictionaryExtnExample &lt;Bob=[e=40,v=2],

//Bob=[e=10,v=1], Jen=[e=25,v=3]&gt;] trace(&quot;Is e_roundtrip a JSONDictionaryExtnExample? &quot; + (e_roundtrip is JSONDictionaryExtnExample)); //true

trace(&quot;Name change: Jen is now Jan&quot;); a_jen.dname = &quot;Jan&quot;

trace(&quot;e: &quot; + e); //[JSONDictionaryExtnExample &lt;Bob=[e=40,v=2], Bob=[e=10,v=1],

//Jan=[e=25,v=3]&gt;]

trace(&quot;e_roundtrip: &quot; + e_roundtrip); //[JSONDictionaryExtnExample &lt;Bob=[e=40,v=2],

//Bob=[e=10,v=1], Jan=[e=25,v=3]&gt;]