## Converting strings between uppercase and lowercase {#converting-strings-between-uppercase-and-lowercase}

Flash Player 9 and later, Adobe AIR 1.0 and later

As the following example shows, the toLowerCase() method and the toUpperCase() method convert alphabetical characters in the string to lowercase and uppercase, respectively:

var str:String = &quot;Dr. Bob Roberts, #9.&quot; trace(str.toLowerCase()); // dr. bob roberts, #9\. trace(str.toUpperCase()); // DR. BOB ROBERTS, #9.

After these methods are executed, the source string remains unchanged. To transform the source string, use the following code:

str = str.toUpperCase();

These methods work with extended characters, not simply a–z and A–Z:

var str:String = &quot;José Barça&quot;;

trace(str.toUpperCase(), str.toLowerCase()); // JOSÉ BARÇA josé barça