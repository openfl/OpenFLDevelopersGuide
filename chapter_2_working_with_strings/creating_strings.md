## Creating strings {#creating-strings}

Flash Player 9 and later, Adobe AIR 1.0 and later

The String class is used to represent string (textual) data in ActionScript 3.0\. ActionScript strings support both ASCII and Unicode characters. The simplest way to create a string is to use a string literal. To declare a string literal, use straight double quotation mark (&quot;) or single quotation mark (&#039;) characters. For example, the following two strings are equivalent:

var str1:String = &quot;hello&quot;; var str2:String = &#039;hello&#039;;

You can also declare a string by using the new operator, as follows:

var str1:String = new String(&quot;hello&quot;); var str2:String = new String(str1);

var str3:String = new String(); // str3 == &quot;&quot;

The following two strings are equivalent:

var str1:String = &quot;hello&quot;;

var str2:String = new String(&quot;hello&quot;);

To use single quotation marks (&#039;) within a string literal defined with single quotation mark (&#039;) delimiters, use the backslash escape character (\). Similarly, to use double quotation marks (&quot;) within a string literal defined with double quotation marks (&quot;) delimiters, use the backslash escape character (\). The following two strings are equivalent:

var str1:String = &quot;That&#039;s \&quot;A-OK\&quot;&quot;; var str2:String = &#039;That\&#039;s &quot;A-OK&quot;&#039;;

You may choose to use single quotation marks or double quotation marks based on any single or double quotation marks that exist in a string literal, as in the following:

var str1:String = &quot;ActionScript &lt;span class=&#039;heavy&#039;&gt;3.0&lt;/span&gt;&quot;; var str2:String = &#039;&lt;item id=&quot;155&quot;&gt;banana&lt;/item&gt;&#039;;

Keep in mind that ActionScript distinguishes between a straight single quotation mark (&#039;) and a left or right single quotation mark (&#039; or &#039; ). The same is true for double quotation marks. Use straight quotation marks to delineate string literals. When pasting text from another source into ActionScript, be sure to use the correct characters.

As the following table shows, you can use the backslash escape character (\) to define other characters in string literals:

| **Escape sequence** | **Character** |
| --- | --- |
| \b | Backspace |
| \f | Form feed |
| \n | Newline |
| \r | Carriage return |
| \t | Tab |
| \u**_nnnn_** | The Unicode character with the character code specified by the hexadecimal number _nnnn_; for example, \u263a is the smiley character. |
| \\x**_nn_** | The ASCII character with the character code specified by the hexadecimal number _nn_ |
| \&#039; | Single quotation mark |
| \&quot; | Double quotation mark |
| \\ | Single backslash character |