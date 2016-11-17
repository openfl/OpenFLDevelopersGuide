## Regular expressions example: A Wiki parser {#regular-expressions-example-a-wiki-parser}

Flash Player 9 and later, Adobe AIR 1.0 and later

This simple Wiki text conversion example illustrates a number of uses for regular expressions:

*   Converting lines of text that match a source Wiki pattern to the appropriate HTML output strings.
*   Using a regular expression to convert URL patterns to HTML &lt;a&gt; hyperlink tags.
*   Using a regular expression to convert U.S. dollar strings (such as &quot;$9.95&quot;) to euro strings (such as &quot;8.24 €&quot;).

To get the application files for this sample, see [www.adobe.com/go/learn_programmingAS3samples_flash](http://www.adobe.com/go/learn_programmingAS3samples_flash). The WikiEditor application files can be found in the folder Samples/WikiEditor. The application consists of the following files:

| **File** | **Description** |
| --- | --- |
| WikiEditor.mxml or | The main application file in Flash (FLA) or Flex (MXML). |
| com/example/programmingas3/regExpExamples/WikiParser.as | A class that includes methods that use regular expressions to convert Wiki input text patterns to the equivalent HTML output. |
| com/example/programmingas3/regExpExamples/URLParser.as | A class that includes methods that use regular expressions to convert URL strings to HTML &lt;a&gt; hyperlink tags. |
| com/example/programmingas3/regExpExamples/CurrencyConverter.as | A class that includes methods that use regular expressions to convert U.S. dollar strings to euro strings. |

**Defining the WikiParser class**

Flash Player 9 and later, Adobe AIR 1.0 and later

The WikiParser class includes methods that convert Wiki input text into the equivalent HTML output. This is not a very robust Wiki conversion application, but it does illustrate some good uses of regular expressions for pattern matching and string conversion.

The constructor function, along with the setWikiData() method, simply initializes a sample string of Wiki input text, as follows:

public function WikiParser()

{

wikiData = setWikiData();

}

When the user clicks the Test button in the sample application, the application invokes the parseWikiString() method of the WikiParser object. This method calls a number of other methods, which in turn assemble the resulting HTML string.

public function parseWikiString(wikiString:String):String

{

var result:String = parseBold(wikiString); result = parseItalic(result);

result = linesToParagraphs(result); result = parseBullets(result); return result;

}

Each of the methods called—parseBold(), parseItalic(), linesToParagraphs(), and parseBullets()—uses the replace() method of the string to replace matching patterns, defined by a regular expression, in order to transform the input Wiki text into HTML-formatted text.

Converting boldface and italic patterns

The parseBold() method looks for a Wiki boldface text pattern (such as &#039;&#039;&#039;foo&#039;&#039;&#039;) and transforms it into its HTML equivalent (such as &lt;b&gt;foo&lt;/b&gt;), as follows:

private function parseBold(input:String):String

{

var pattern:RegExp = /&#039;&#039;&#039;(.*?)&#039;&#039;&#039;/g;

return input.replace(pattern, &quot;&lt;b&gt;$1&lt;/b&gt;&quot;);

}

Note that the (.?*) portion of the regular expression matches any number of characters (*) between the two defining &#039;&#039;&#039; patterns. The ? quantifier makes the match nongreedy, so that for a string such as &#039;&#039;&#039;aaa&#039;&#039;&#039; bbb &#039;&#039;&#039;ccc&#039;&#039;&#039;, the first matched string will be &#039;&#039;&#039;aaa&#039;&#039;&#039; and not the entire string (which starts and ends with the &#039;&#039;&#039; pattern).

The parentheses in the regular expression define a capturing group, and the replace() method refers to this group by using the $1 code in the replacement string. The g (global) flag in the regular expression ensures that the replace() method replaces all matches in the string (not simply the first one).

The parseItalic() method works similarly to the parseBold() method, except that it checks for two apostrophes (&#039;&#039;) as the delimiter for italic text (not three):

private function parseItalic(input:String):String

{

var pattern:RegExp = /&#039;&#039;(.*?)&#039;&#039;/g;

return input.replace(pattern, &quot;&lt;i&gt;$1&lt;/i&gt;&quot;);

}

Converting bullet patterns

As the following example shows, the parseBullet() method looks for the Wiki bullet line pattern (such as * foo) and transforms it into its HTML equivalent (such as &lt;li&gt;foo&lt;/li&gt;):

private function parseBullets(input:String):String

{

var pattern:RegExp = /^\*(.*)/gm;

return input.replace(pattern, &quot;&lt;li&gt;$1&lt;/li&gt;&quot;);

}

The ^ symbol at the beginning of the regular expression matches the beginning of a line. The m (multiline) flag in the regular expression causes the regular expression to match the ^ symbol against the start of a line, not simply the start of the string.

The \* pattern matches an asterisk character (the backslash is used to signal a literal asterisk instead of a * quantifier).

The parentheses in the regular expression define a capturing group, and the replace() method refers to this group by using the $1 code in the replacement string. The g (global) flag in the regular expression ensures that the replace() method replaces all matches in the string (not simply the first one).

Converting paragraph Wiki patterns

The linesToParagraphs() method converts each line in the input Wiki string to an HTML &lt;p&gt; paragraph tag. These lines in the method strip out empty lines from the input Wiki string:

var pattern:RegExp = /^$/gm;

var result:String = input.replace(pattern, &quot;&quot;);

The ^ and $ symbols the regular expression match the beginning and end of a line. The m (multiline) flag in the regular expression causes the regular expression to match the ^ symbol against the start of a line, not simply the start of the string.

The replace() method replaces all matching substrings (empty lines) with an empty string (&quot;&quot;). The g (global) flag in the regular expression ensures that the replace() method replaces all matches in the string (not simply the first one).

### Converting URLs to HTML &lt;a&gt; tags {#converting-urls-to-html-a-tags}

Flash Player 9 and later, Adobe AIR 1.0 and later

When the user clicks the Test button in the sample application, if the user selected the urlToATag check box, the application calls the URLParser.urlToATag() static method to convert URL strings from the input Wiki string into HTML &lt;a&gt; tags.

var protocol:String = &quot;((?:http|ftp)://)&quot;;

var urlPart:String = &quot;([a-z0-9_-]+\.[a-z0-9_-]+)&quot;; var optionalUrlPart:String = &quot;(\.[a-z0-9_-]*)&quot;;

var urlPattern:RegExp = new RegExp(protocol + urlPart + optionalUrlPart, &quot;ig&quot;);

var result:String = input.replace(urlPattern, &quot;&lt;a href=&#039;$1$2$3&#039;&gt;&lt;u&gt;$1$2$3&lt;/u&gt;&lt;/a&gt;&quot;);

The RegExp() constructor function is used to assemble a regular expression (urlPattern) from a number of constituent parts. These constituent parts are each strings that define part of the regular expression pattern.

The first part of the regular expression pattern, defined by the protocol string, defines an URL protocol: either http:// or ftp://. The parentheses define a noncapturing group, indicated by the ? symbol. This means that the parentheses are simply used to define a group for the | alternation pattern; the group will not match backreference codes ($1, $2, $3) in the replacement string of the replace() method.

The other constituent parts of the regular expression each use capturing groups (indicated by parentheses in the pattern), which are then used in the backreference codes ($1, $2, $3) in the replacement string of the replace() method.

The part of the pattern defined by the urlPart string matches _at least_ one of the following characters: a-z, 0-9, _, or

-. The + quantifier indicates that at least one character is matched. The \. indicates a required dot (.) character. And the remainder matches another string of at least one of these characters: a-z, 0-9, _, or -.

The part of the pattern defined by the optionalUrlPart string matches _zero or more_ of the following: a dot (.) character followed by any number of alphanumeric characters (including _ and -). The * quantifier indicates that zero or more characters are matched.

The call to the replace() method employs the regular expression and assembles the replacement HTML string, using backreferences.

The urlToATag() method then calls the emailToATag() method, which uses similar techniques to replace e-mail patterns with HTML &lt;a&gt; hyperlink strings. The regular expressions used to match HTTP, FTP, and e-mail URLs in this sample file are fairly simple, for the purposes of exemplification; there are much more complicated regular expressions for matching such URLs more correctly.

### Converting U.S. dollar strings to euro strings {#converting-u-s-dollar-strings-to-euro-strings}

Flash Player 9 and later, Adobe AIR 1.0 and later

When the user clicks the Test button in the sample application, if the user selected the dollarToEuro check box, the application calls the CurrencyConverter.usdToEuro() static method to convert U.S. dollar strings (such as &quot;$9.95&quot;) to euro strings (such as &quot;8.24 €&quot;), as follows:

var usdPrice:RegExp = /\$([\d,]+.\d+)+/g;

return input.replace(usdPrice, usdStrToEuroStr);

The first line defines a simple pattern for matching U.S. dollar strings. Notice that the $ character is preceded with the backslash (\) escape character.

The replace() method uses the regular expression as the pattern matcher, and it calls the usdStrToEuroStr()

function to determine the replacement string (a value in euros).

When a function name is used as the second parameter of the replace() method, the following are passed as parameters to the called function:

*   The matching portion of the string.
*   Any captured parenthetical group matches. The number of arguments passed this way varies depending on the number of captured parenthetical group matches. You can determine the number of captured parenthetical group matches by checking arguments.length - 3 within the function code.
*   The index position in the string where the match begins.
*   The complete string.

The usdStrToEuroStr() method converts U.S. dollar string patterns to euro strings, as follows:

private function usdToEuro(...args):String

{

var usd:String = args[1]; usd = usd.replace(&quot;,&quot;, &quot;&quot;);

var exchangeRate:Number = 0.828017;

var euro:Number = Number(usd) * exchangeRate; trace(usd, Number(usd), euro);

const euroSymbol:String = String.fromCharCode(8364); // € return euro.toFixed(2) + &quot; &quot; + euroSymbol;

}

Note that args[1] represents the captured parenthetical group matched by the usdPrice regular expression. This is the numerical portion of the U.S. dollar string: that is, the dollar amount without the $ sign. The method applies an exchange rate conversion and returns the resulting string (with a trailing € symbol instead of a leading $ symbol).