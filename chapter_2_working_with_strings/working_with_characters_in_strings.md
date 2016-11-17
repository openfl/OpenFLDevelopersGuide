## Working with characters in strings {#working-with-characters-in-strings}

Flash Player 9 and later, Adobe AIR 1.0 and later

Every character in a string has an index position in the string (an integer). The index position of the first character is

0\. For example, in the following string, the character y is in position 0 and the character w is in position 5:

&quot;yellow&quot;

You can examine individual characters in various positions in a string using the charAt() method and the

charCodeAt() method, as in this example:

var str:String = &quot;hello world!&quot;;

for (var i:int = 0; i &lt; str.length; i++)

{

trace(str.charAt(i), &quot;-&quot;, str.charCodeAt(i));

}

When you run this code, the following output is produced:

| h | - | 104 |
| --- | --- | --- |
| e | - | 101 |
| l | - | 108 |
| l | - | 108 |
| o | - | 111 |
| - | 32 |  |
| w | - | 119 |
| o | - | 111 |
| r | - | 114 |
| l | - | 108 |
| d | - | 100 |
| ! | - | 33 |

You can also use character codes to define a string using the fromCharCode() method, as the following example shows:

var myStr:String = String.fromCharCode(104,101,108,108,111,32,119,111,114,108,100,33);

// Sets myStr to &quot;hello world!&quot;