## The length property {#the-length-property}

Flash Player 9 and later, Adobe AIR 1.0 and later

Every string has a length property, which is equal to the number of characters in the string:

var str:String = &quot;Adobe&quot;; trace(str.length); // output: 5

An empty string and a null string both have a length of 0, as the following example shows:

var str1:String = new String(); trace(str1.length); // output: 0

str2:String = &#039;&#039;;

trace(str2.length); // output: 0