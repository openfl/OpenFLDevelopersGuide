## Concatenating strings {#concatenating-strings}

Flash Player 9 and later, Adobe AIR 1.0 and later

Concatenation of strings means taking two strings and joining them sequentially into one. For example, you can use the + operator to concatenate two strings:

var str1:String = &quot;green&quot;; var str2:String = &quot;ish&quot;;

var str3:String = str1 + str2; // str3 == &quot;greenish&quot;

You can also use the += operator to the produce the same result, as the following example shows:

var str:String = &quot;green&quot;;

str += &quot;ish&quot;; // str == &quot;greenish&quot;

Additionally, the String class includes a concat() method, which can be used as follows:

var str1:String = &quot;Bonjour&quot;; var str2:String = &quot;from&quot;; var str3:String = &quot;Paris&quot;;

var str4:String = str1.concat(&quot; &quot;, str2, &quot; &quot;, str3);

// str4 == &quot;Bonjour from Paris&quot;

If you use the + operator (or the += operator) with a String object and an object that is _not_ a string, ActionScript automatically converts the nonstring object to a String object in order to evaluate the expression, as shown in this example:

var str:String = &quot;Area = &quot;;

var area:Number = Math.PI * Math.pow(3, 2);

str = str + area; // str == &quot;Area = 28.274333882308138&quot;

However, you can use parentheses for grouping to provide context for the + operator, as the following example shows:

trace(&quot;Total: $&quot; + 4.55 + 1.45); // output: Total: $4.551.45 trace(&quot;Total: $&quot; + (4.55 + 1.45)); // output: Total: $6