## Comparing strings {#comparing-strings}

Flash Player 9 and later, Adobe AIR 1.0 and later

You can use the following operators to compare strings: &lt;, &lt;=, !=, ==, =&gt;, and &gt;. These operators can be used with conditional statements, such as if and while, as the following example shows:

var str1:String = &quot;Apple&quot;; var str2:String = &quot;apple&quot;; if (str1 &lt; str2)

{

trace(&quot;A &lt; a, B &lt; b, C &lt; c, ...&quot;);

}

When using these operators with strings, ActionScript considers the character code value of each character in the string, comparing characters from left to right, as in the following:

trace(&quot;A&quot; &lt; &quot;B&quot;); // true

trace(&quot;A&quot; &lt; &quot;a&quot;); // true

trace(&quot;Ab&quot; &lt; &quot;az&quot;); // true trace(&quot;abc&quot; &lt; &quot;abza&quot;); // true

Use the == and != operators to compare strings with each other and to compare strings with other types of objects, as the following example shows:

var str1:String = &quot;1&quot;; var str1b:String = &quot;1&quot;; var str2:String = &quot;2&quot;;

trace(str1 == str1b); // true trace(str1 == str2); // false var total:uint = 1; trace(str1 == total); // true