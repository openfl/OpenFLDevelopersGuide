# Chapter 41: Working with byte arrays {#chapter-41-working-with-byte-arrays}

The [ByteArray class](https://api.openfl.org/openfl/utils/ByteArray.html) allows
you to read from and write to a binary stream of data, which is essentially an
array of bytes. This class provides a way to access data at the most elemental
level. Because computer data consists of bytes, or groups of 8 bits, the ability
to read data in bytes means that you can access data for which classes and
access methods do not exist. The ByteArray class allows you to parse any stream
of data, from a bitmap to a stream of data traveling over the network, at the
byte level.

The `writeObject()` method allows you to write an object in serialized format to
a ByteArray, while the `readObject()` method allows you to read a serialized
object from a ByteArray to a variable of the original data type. You can
serialize any object except for display objects, which are those objects that
can be placed on the display list. You can also assign serialized objects back
to custom class instances if the custom class is available to the runtime. After
serializing an object to bytes, you can efficiently transfer it over a network
connection or save it to a file.

> Objects in Adobe Flash were always serialized using _Action Message Format_
> (abbreviated as AMF). OpenFL supports a number of serialization formats,
> including:
> 
> - [Haxe Serialization Format](https://haxe.org/manual/std-serialization.html)
> - [JSON](https://en.wikipedia.org/wiki/JSON)
> - [Action Message Format (AMF)](https://en.wikipedia.org/wiki/Action_Message_Format)

## Section Contents

- [Reading and writing a byte array](./reading-and-writing-a-byte-array.md)

<!-- TODO: uncomment when this content is adapted for OpenFL
- [ByteArray example: Reading a .zip file](./bytearray-example-reading-a-zip-file.md)-->

## More Help topics

- [openfl.utils.ByteArray](https://api.openfl.org/openfl/utils/ByteArray.html)
- [openfl.utils.IExternalizable](https://api.openfl.org/openfl/utils/IExternalizable.html)
