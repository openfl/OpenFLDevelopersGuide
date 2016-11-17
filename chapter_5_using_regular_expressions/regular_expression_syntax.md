## Regular expression syntax {#regular-expression-syntax}

Flash Player 9 and later, Adobe AIR 1.0 and later

This section describes all of the elements of ActionScript regular expression syntax. As you’ll see, regular expressions can have many complexities and nuances. You can find detailed resources on regular expressions on the web and in bookstores. Keep in mind that different programming environments implement regular expressions in different ways. ActionScript 3.0 implements regular expressions as defined in the ECMAScript edition 3 language specification (ECMA-262).

Generally, you use regular expressions that match more complicated patterns than a simple string of characters. For example, the following regular expression defines the pattern consisting of the letters A, B, and C in sequence followed by any digit:

/ABC\d/

The \d code represents “any digit.” The backslash (\) character is called the escape character, and combined with the character that follows it (in this case the letter d), it has special meaning in the regular expression.

The following regular expression defines the pattern of the letters ABC followed by any number of digits (note the asterisk):

/ABC\d*/

The asterisk character (*) is a _metacharacter_. A metacharacter is a character that has special meaning in regular expressions. The asterisk is a specific type of metacharacter called a _quantifier,_ which is used to quantify the amount of repetition of a character or group of characters. For more information, see

“Quantifiers” on page 82

.

In addition to its pattern, a regular expression can contain flags, which specify how the regular expression is to be matched. For example, the following regular expression uses the i flag, which specifies that the regular expression ignores case sensitivity in matching strings:

/ABC\d*/i

For more information, see

“Flags and properties” on page 87

.

You can use regular expressions with the following methods of the String class: match(), replace(), and search(). For more information on these methods, see

“Finding patterns in strings and replacing substrings” on page 16

.

**Creating an instance of a regular expression**

Flash Player 9 and later, Adobe AIR 1.0 and later

There are two ways to create a regular expression instance. One way uses forward slash characters (/) to delineate the regular expression; the other uses the new constructor. For example, the following regular expressions are equivalent:

var pattern1:RegExp = /bob/i;

var pattern2:RegExp = new RegExp(&quot;bob&quot;, &quot;i&quot;);

Forward slashes delineate a regular expression literal in the same way as quotation marks delineate a string literal. The part of the regular expression within the forward slashes defines the _pattern._ The regular expression can also include _flags_ after the final delineating slash. These flags are considered to be part of the regular expression, but they are separate from its pattern.

When using the new constructor, you use two strings to define the regular expression. The first string defines the pattern, and the second string defines the flags, as in the following example:

var pattern2:RegExp = new RegExp(&quot;bob&quot;, &quot;i&quot;);

When including a forward slash _within_ a regular expression that is defined by using the forward slash delineators, you must precede the forward slash with the backslash (\) escape character. For example, the following regular expression matches the pattern 1/2:

var pattern:RegExp = /1\/2/;

To include quotation marks _within_ a regular expression that is defined with the new constructor, you must add backslash (\) escape character before the quotation marks (just as you would when defining any String literal). For example, the following regular expressions match the pattern eat at &quot;joe&#039;s&quot;:

var pattern1:RegExp = new RegExp(&quot;eat at \&quot;joe&#039;s\&quot;&quot;, &quot;&quot;); var pattern2:RegExp = new RegExp(&#039;eat at &quot;joe\&#039;s&quot;&#039;, &quot;&quot;);

Do not use the backslash escape character with quotation marks in regular expressions that are defined by using the forward slash delineators. Similarly, do not use the escape character with forward slashes in regular expressions that are defined with the new constructor. The following regular expressions are equivalent, and they define the pattern 1/2 &quot;joe&#039;s&quot;:

var pattern1:RegExp = /1\/2 &quot;joe&#039;s&quot;/;

var pattern2:RegExp = new RegExp(&quot;1/2 \&quot;joe&#039;s\&quot;&quot;, &quot;&quot;); var pattern3:RegExp = new RegExp(&#039;1/2 &quot;joe\&#039;s&quot;&#039;, &#039;&#039;);

Also, in a regular expression that is defined with the new constructor, to use a metasequence that begins with the backslash (\) character, such as \d (which matches any digit), type the backslash character twice:

var pattern:RegExp = new RegExp(&quot;\\d+&quot;, &quot;&quot;); // matches one or more digits

You must type the backlash character twice in this case, because the first parameter of the RegExp() constructor method is a string, and in a string literal you must type a backslash character twice to have it recognized as a single backslash character.

The sections that follow describe syntax for defining regular expression patterns. For more information on flags, see

“Flags and properties” on page 87

.

### Characters, metacharacters, and metasequences {#characters-metacharacters-and-metasequences}

Flash Player 9 and later, Adobe AIR 1.0 and later

The simplest regular expression is one that matches a sequence of characters, as in the following example:

var pattern:RegExp = /hello/;

However, the following characters, known as metacharacters_,_ have special meanings in regular expressions:

^ $ \ . * + ? ( ) [ ] { } |

For example, the following regular expression matches the letter A followed by zero or more instances of the letter B (the asterisk metacharacter indicates this repetition), followed by the letter C:

/AB*C/

To include a metacharacter without its special meaning in a regular expression pattern, you must use the backslash (\) escape character. For example, the following regular expression matches the letter A followed by the letter B, followed by an asterisk, followed by the letter C:

var pattern:RegExp = /AB\*C/;

A _metasequence,_ like a metacharacter, has special meaning in a regular expression. A metasequence is made up of more than one character. The following sections provide details on using metacharacters and metasequences.

About metacharacters

The following table summarizes the metacharacters that you can use in regular expressions:

| **Metacharacter** | **Description** |
| --- | --- |
| ^ (caret) | Matches at the start of the string. With the m (multiline) flag set, the caret matches the start of a line as well (see

“Flags and properties” on page 87

). Note that when used at the start of a character class, the caret indicates negation, not the start of a string. For more information, see

“Character classes” on page 81

. |
| $(dollar sign) | Matches at the end of the string. With the m (multiline) flag set, $ matches the position before a newline (\n) character as well. For more information, see

“Flags and properties” on page 87

. |
| \ (backslash) | Escapes the special metacharacter meaning of special characters. |
| . (dot) | Matches any single character. |
| * (star) | Matches the previous item repeated zero or more times. For more information, see

“Quantifiers” on page 82

. |
| + (plus) | Matches the previous item repeated one or more times. For more information, see

“Quantifiers” on page 82

. |
| ? (question mark) | Matches the previous item repeated zero times or one time. For more information, see

“Quantifiers” on page 82

. |

| **Metacharacter** | **Description** |
| --- | --- |
| ( and ) | Defines groups within the regular expression. Use groups for the following: |
| [ and ] | Defines a character class, which defines possible matches for a single character: |
| | _(pipe)_ | Used for alternation, to match either the part on the left side or the part on the right side: |

About metasequences

Metasequences are sequences of characters that have special meaning in a regular expression pattern. The following table describes these metasequences:

| **Metasequence** | **Description** |
| --- | --- |
| {**_n_**} | Specifies a numeric quantifier or quantifier range for the previous item: |
| \b | Matches at the position between a word character and a nonword character. If the first or last character in the string is a word character, also matches the start or end of the string. |
| \B | Matches at the position between two word characters. Also matches the position between two nonword characters. |
| \d | Matches a decimal digit. |
| \D | Matches any character other than a digit. |
| \f | Matches a form feed character. |
| \n | Matches the newline character. |

| **Metasequence** | **Description** |
| --- | --- |
| \r | Matches the return character. |
| \s | Matches any white-space character (a space, tab, newline, or return character). |
| \S | Matches any character other than a white-space character. |
| \t | Matches the tab character. |
| \u**_nnnn_** | Matches the Unicode character with the character code specified by the hexadecimal number _nnnn_. For example, \u263a is the smiley character. |
| \v | Matches a vertical feed character. |
| \w | Matches a word character (AZ–, az–, 0-9, or _). Note that \w does not match non-English characters, such as é , ñ , or ç . |
| \W | Matches any character other than a word character. |
| \\x**_nn_** | Matches the character with the specified ASCII value, as defined by the hexadecimal number _nn_. |

### Character classes {#character-classes}

Flash Player 9 and later, Adobe AIR 1.0 and later

You use character classes to specify a list of characters to match one position in the regular expression. You define character classes with square brackets ( [ and ] ). For example, the following regular expression defines a character class that matches bag, beg, big, bog, or bug:

/b[aeiou]g/

Escape sequences in character classes

Most metacharacters and metasequences that normally have special meanings in a regular expression _do not_ have those same meanings inside a character class. For example, in a regular expression, the asterisk is used for repetition, but this is not the case when the asterisk appears in a character class. The following character class matches the asterisk literally, along with any of the other characters listed:

/[abc*123]/

However, the three characters listed in the following table do function as metacharacters, with special meaning, in character classes:

| **Metacharacter** | **Meaning in character classes** |
| --- | --- |
| ] | Defines the end of the character class. |
| - | Defines a range of characters (see the following section “Ranges of characters in character classes”). |
| \ | Defines metasequences and undoes the special meaning of metacharacters. |

For any of these characters to be recognized as literal characters (without the special metacharacter meaning), you must precede the character with the backslash escape character. For example, the following regular expression includes a character class that matches any one of four symbols ($, \, ], or -):

/[$\\\]\-]/

In addition to the metacharacters that retain their special meanings, the following metasequences function as metasequences within character classes:

| **Metasequence** | **Meaning in character classes** |
| --- | --- |
| \n | Matches a newline character. |
| \r | Matches a return character. |
| \t | Matches a tab character. |
| \u**_nnnn_** | Matches the character with the specified Unicode code point value (as defined by the hexadecimal number |
| \\x**_nn_** | Matches the character with the specified ASCII value (as defined by the hexadecimal number _nn_). |

Other regular expression metasequences and metacharacters are treated as normal characters within a character class.

Ranges of characters in character classes

Use the hyphen to specify a range of characters, such as A-Z, a-z, or 0-9\. These characters must constitute a valid range in the character set. For example, the following character class matches any one of the characters in the range a-z or any digit:

/[a-z0-9]/

You can also use the \\x_nn_ ASCII character code to specify a range by ASCII value. For example, the following character class matches any character from a set of extended ASCII characters (such as é and ê ):

\\x

Negated character classes

When you use a caret (^) character at the beginning of a character class, it negates that class—any character not listed is considered a match. The following character class matches any character _except_ for a lowercase letter (az–) or a digit:

/[^a-z0-9]/

You must type the caret (^) character at the _beginning_ of a character class to indicate negation. Otherwise, you are simply adding the caret character to the characters in the character class. For example, the following character class matches any one of a number of symbol characters, including the caret:

/[!.,#+*%$&amp;^]/

### Quantifiers {#quantifiers}

Flash Player 9 and later, Adobe AIR 1.0 and later

You use quantifiers to specify repetitions of characters or sequences in patterns, as follows:

| **Quantifier metacharacter** | **Description** |
| --- | --- |
| * (star) | Matches the previous item repeated zero or more times. |
| + (plus) | Matches the previous item repeated one or more times. |
| ? (question mark) | Matches the previous item repeated zero times or one time. |
| {**_n_**} | Specifies a numeric quantifier or quantifier range for the previous item: |

You can apply a quantifier to a single character, to a character class, or to a group:

*   /a+/ matches the character a repeated one or more times.
*   /\d+/ matches one or more digits.
*   /[abc]+/ matches a repetition of one or more character, each of which is either a, b, or c.
*   /(very, )*/ matches the word very followed by a comma and a space repeated zero or more times.

You can use quantifiers within parenthetical groupings that have quantifiers applied to them. For example, the following quantifier matches strings such as word and word-word-word:

/\w+(-\w+)*/

By default, regular expressions perform what is known as _greedy matching._ Any subpattern in the regular expression (such as .*) tries to match as many characters in the string as possible before moving forward to the next part of the regular expression. For example, consider the following regular expression and string:

var pattern:RegExp = /&lt;p&gt;.*&lt;\/p&gt;/;

str:String = &quot;&lt;p&gt;Paragraph 1&lt;/p&gt; &lt;p&gt;Paragraph 2&lt;/p&gt;&quot;;

The regular expression matches the entire string:

&lt;p&gt;Paragraph 1&lt;/p&gt; &lt;p&gt;Paragraph 2&lt;/p&gt;

Suppose, however, that you want to match only one &lt;p&gt;...&lt;/p&gt; grouping. You can do this with the following:

&lt;p&gt;Paragraph 1&lt;/p&gt;

Add a question mark (?) after any quantifier to change it to what is known as a _lazy quantifier_. For example, the following regular expression, which uses the lazy *? quantifier, matches &lt;p&gt; followed by the minimum number of characters possible (lazy), followed by &lt;/p&gt;:

/&lt;p&gt;.*?&lt;\/p&gt;/

Keep in mind the following points about quantifiers:

*   The quantifiers {0} and {0,0} do not exclude an item from a match.
*   Do not combine multiple quantifiers, as in /abc+*/.
*   The dot (.) does not span lines unless the s (dotall) flag is set, even if it is followed by a * quantifier. For example, consider the following code:

var str:String = &quot;&lt;p&gt;Test\n&quot;; str += &quot;Multiline&lt;/p&gt;&quot;;

var re:RegExp = /&lt;p&gt;.*&lt;\/p&gt;/; trace(str.match(re)); // null;

re = /&lt;p&gt;.*&lt;\/p&gt;/s; trace(str.match(re));

// output: &lt;p&gt;Test

// Multiline&lt;/p&gt;

For more information, see

“Flags and properties” on page 87

.

### Alternation {#alternation}

Flash Player 9 and later, Adobe AIR 1.0 and later

Use the | (pipe) character in a regular expression to have the regular expression engine consider alternatives for a match. For example, the following regular expression matches any one of the words cat, dog, pig, rat:

var pattern:RegExp = /cat|dog|pig|rat/;

You can use parentheses to define groups to restrict the scope of the | alternator. The following regular expression matches cat followed by nap or nip:

var pattern:RegExp = /cat(nap|nip)/;

For more information, see

“Groups” on page 84

.

The following two regular expressions, one using the | alternator, the other using a character class (defined with [ and

] ), are equivalent:

/1|3|5|7|9/

/[13579]/

For more information, see

“Character classes” on page 81

.

### Groups {#groups}

Flash Player 9 and later, Adobe AIR 1.0 and later

You can specify a group in a regular expression by using parentheses, as follows:

/class-(\d*)/

A group is a subsection of a pattern. You can use groups to do the following things:

*   Apply a quantifier to more than one character.
*   Delineate subpatterns to be applied with alternation (by using the | character).
*   Capture substring matches (for example, by using \1 in a regular expression to match a previously matched group, or by using $1 similarly in the replace() method of the String class).

The following sections provide details on these uses of groups.

Using groups with quantifiers

If you do not use a group, a quantifier applies to the character or character class that precedes it, as the following shows:

var pattern:RegExp = /ab*/ ;

// matches the character a followed by

// zero or more occurrences of the character b

pattern = /a\d+/;

// matches the character a followed by

// one or more digits

pattern = /a[123]{1,3}/;

// matches the character a followed by

// one to three occurrences of either 1, 2, or 3

However, you can use a group to apply a quantifier to more than one character or character class:

var pattern:RegExp = /(ab)*/;

// matches zero or more occurrences of the character a

// followed by the character b, such as ababab

pattern = /(a\d)+/;

// matches one or more occurrences of the character a followed by

// a digit, such as a1a5a8a3

pattern = /(spam ){1,3}/;

// matches 1 to 3 occurrences of the word spam followed by a space

For more information on quantifiers, see

“Quantifiers” on page 82

.

Using groups with the alternator (|) character

You can use groups to define the group of characters to which you want to apply an alternator (|) character, as follows:

var pattern:RegExp = /cat|dog/;

// matches cat or dog

pattern = /ca(t|d)og/;

// matches catog or cadog

Using groups to capture substring matches

When you define a standard parenthetical group in a pattern, you can later refer to it in the regular expression. This is known as a _backreference_, and these sorts of groups are known as _capturing groups_. For example, in the following regular expression, the sequence \1 matches whatever substring matched the capturing parenthetical group:

var pattern:RegExp = /(\d+)-by-\1/;

// matches the following: 48-by-48

You can specify up to 99 of these backreferences in a regular expression by typing \1, \2, ... , \99.

Similarly, in the replace() method of the String class, you can use $1$99– to insert captured group substring matches in the replacement string:

var pattern:RegExp = /Hi, (\w+)\./; var str:String = &quot;Hi, Bob.&quot;;

trace(str.replace(pattern, &quot;$1, hello.&quot;));

// output: Bob, hello.

Also, if you use capturing groups, the exec() method of the RegExp class and the match() method of the String class return substrings that match the capturing groups:

var pattern:RegExp = /(\w+)@(\w+).(\w+)/; var str:String = &quot;bob@example.com&quot;; trace(pattern.exec(str));

// bob@example.com,bob,example,com

Using noncapturing groups and lookahead groups

A noncapturing group is one that is used for grouping only; it is not “collected,” and it does not match numbered backreferences. Use (?: and ) to define noncapturing groups, as follows:

var pattern = /(?:com|org|net);

For example, note the difference between putting (com|org) in a capturing versus a noncapturing group (the exec()

method lists capturing groups after the complete match):

var pattern:RegExp = /(\w+)@(\w+).(com|org)/; var str:String = &quot;bob@example.com&quot;; trace(pattern.exec(str));

// bob@example.com,bob,example,com

//noncapturing:

var pattern:RegExp = /(\w+)@(\w+).(?:com|org)/; var str:String = &quot;bob@example.com&quot;; trace(pattern.exec(str));

// bob@example.com,bob,example

A special type of noncapturing group is the _lookahead group,_ of which there are two types: the _positive lookahead group_

and the _negative lookahead group._

Use (?= and ) to define a positive lookahead group, which specifies that the subpattern in the group must match at the position. However, the portion of the string that matches the positive lookahead group can match remaining patterns in the regular expression. For example, because (?=e) is a positive lookahead group in the following code, the character e that it matches can be matched by a subsequent part of the regular expression—in this case, the capturing group, \w*):

var pattern:RegExp = /sh(?=e)(\w*)/i;

var str:String = &quot;Shelly sells seashells by the seashore&quot;; trace(pattern.exec(str));

// Shelly,elly

Use (?! and ) to define a negative lookahead group that specifies that the subpattern in the group must _not_ match at the position. For example:

var pattern:RegExp = /sh(?!e)(\w*)/i;

var str:String = &quot;She sells seashells by the seashore&quot;; trace(pattern.exec(str));

// shore,ore

Using named groups

A named group is a type of group in a regular expression that is given a named identifier. Use (?P&lt;name&gt; and ) to define the named group. For example, the following regular expression includes a named group with the identifier named digits:

var pattern = /[a-z]+(?P&lt;digits&gt;\d+)[a-z]+/;

When you use the exec() method, a matching named group is added as a property of the result array:

var myPattern:RegExp = /([a-z]+)(?P&lt;digits&gt;\d+)[a-z]+/; var str:String = &quot;a123bcd&quot;;

var result:Array = myPattern.exec(str); trace(result.digits); // 123

Here is another example, which uses two named groups, with the identifiers name and dom:

var emailPattern:RegExp =

/(?P&lt;name&gt;(\w|[_.\-])+)@(?P&lt;dom&gt;((\w|-)+))+\.\w{2,4}+/; var address:String = &quot;bob@example.com&quot;;

var result:Array = emailPattern.exec(address); trace(result.name); // bob

trace(result.dom); // example

**_Note:_ **_Named groups are not part of the ECMAScript language specification. They are an added feature in ActionScript 3.0._

### Flags and properties {#flags-and-properties}

Flash Player 9 and later, Adobe AIR 1.0 and later

The following table lists the five flags that you can set for regular expressions. Each flag can be accessed as a property of the regular expression object.

| **Flag** | **Property** | **Description** |
| --- | --- | --- |
| g | global | Matches more than one match. |
| i | ignoreCase | Case-insensitive matching. Applies to the A—Z and a—z characters, but not to extended characters such as É and é . |
| m | multiline | With this flag set, $ and ^ can match the beginning of a line and end of a line, respectively. |
| s | dotall | With this flag set, . (dot) can match the newline character (\n). |
| x | extended | Allows extended regular expressions. You can type spaces in the regular expression, which are ignored as part of the pattern. This lets you type regular expression code more legibly. |

Note that these properties are read-only. You can set the flags (g, i, m, s, x) when you set a regular expression variable, as follows:

var re:RegExp = /abc/gimsx;

However, you cannot directly set the named properties. For instance, the following code results in an error:

var re:RegExp = /abc/;

re.global = true; // This generates an error.

By default, unless you specify them in the regular expression declaration, the flags are not set, and the corresponding properties are also set to false.

Additionally, there are two other properties of a regular expression:

*   The lastIndex property specifies the index position in the string to use for the next call to the exec() or test()

method of a regular expression.

*   The source property specifies the string that defines the pattern portion of the regular expression.

The g (global) flag

When the g (global) flag is _not_ included, a regular expression matches no more than one match. For example, with the g flag not included in the regular expression, the String.match() method returns only one matching substring:

var str:String = &quot;she sells seashells by the seashore.&quot;; var pattern:RegExp = /sh\w*/;

trace(str.match(pattern)) // output: she

When the g flag is set, the Sting.match() method returns multiple matches, as follows:

var str:String = &quot;she sells seashells by the seashore.&quot;; var pattern:RegExp = /sh\w*/g;

// The same pattern, but this time the g flag IS set. trace(str.match(pattern)); // output: she,shells,shore

The i (ignoreCase) flag

By default, regular expression matches are case-sensitive. When you set the i (ignoreCase) flag, case sensitivity is ignored. For example, the lowercase s in the regular expression does not match the uppercase letter S, the first character of the string:

var str:String = &quot;She sells seashells by the seashore.&quot;; trace(str.search(/sh/)); // output: 13 -- Not the first character

With the i flag set, however, the regular expression does match the capital letter S:

var str:String = &quot;She sells seashells by the seashore.&quot;; trace(str.search(/sh/i)); // output: 0

The i flag ignores case sensitivity only for the A–Z and a–z characters, but not for extended characters such as É and é .

The m (multiline) flag

If the m (multiline) flag is not set, the ^ matches the beginning of the string and the $ matches the end of the string. If the m flag is set, these characters match the beginning of a line and end of a line, respectively. Consider the following string, which includes a newline character:

var str:String = &quot;Test\n&quot;; str += &quot;Multiline&quot;;

trace(str.match(/^\w*/g)); // Match a word at the beginning of the string.

Even though the g (global) flag is set in the regular expression, the match() method matches only one substring, since there is only one match for the ^—the beginning of the string. The output is:

Test

Here is the same code with the m flag set:

var str:String = &quot;Test\n&quot;; str += &quot;Multiline&quot;;

trace(str.match(/^\w*/gm)); // Match a word at the beginning of lines. This time, the output includes the words at the beginning of both lines: Test,Multiline

Note that only the \n character signals the end of a line. The following characters do not:

*   Return (\r) character
*   Unicode line-separator (\u2028) character
*   Unicode paragraph-separator (\u2029) character

The s (dotall) flag

If the s (dotall or “dot all”) flag is not set, a dot (.) in a regular expression pattern does not match a newline character (\n). So for the following example, there is no match:

var str:String = &quot;&lt;p&gt;Test\n&quot;; str += &quot;Multiline&lt;/p&gt;&quot;;

var re:RegExp = /&lt;p&gt;.*?&lt;\/p&gt;/; trace(str.match(re));

However, if the s flag is set, the dot matches the newline character:

var str:String = &quot;&lt;p&gt;Test\n&quot;; str += &quot;Multiline&lt;/p&gt;&quot;;

var re:RegExp = /&lt;p&gt;.*?&lt;\/p&gt;/s; trace(str.match(re));

In this case, the match is the entire substring within the &lt;p&gt; tags, including the newline character:

&lt;p&gt;Test Multiline&lt;/p&gt;

The x (extended) flag

Regular expressions can be difficult to read, especially when they include a lot of metasymbols and metasequences. For example:

/&lt;p(&gt;|(\s*[^&gt;]*&gt;)).*?&lt;\/p&gt;/gi

When you use the x (extended) flag in a regular expression, any blank spaces that you type in the pattern are ignored. For example, the following regular expression is identical to the previous example:

/ &lt;p (&gt; | (\s* [^&gt;]* &gt;)) .*? &lt;\/p&gt; /gix

If you have the x flag set and do want to match a blank space character, precede the blank space with a backslash. For example, the following two regular expressions are equivalent:

/foo bar/

/foo \ bar/x

The lastIndex property

The lastIndex property specifies the index position in the string at which to start the next search. This property affects the exec() and test() methods called on a regular expression that has the g flag set to true. For example, consider the following code:

var pattern:RegExp = /p\w*/gi;

var str:String = &quot;Pedro Piper picked a peck of pickled peppers.&quot;; trace(pattern.lastIndex);

var result:Object = pattern.exec(str); while (result != null)

{

trace(pattern.lastIndex); result = pattern.exec(str);

}

The lastIndex property is set to 0 by default (to start searches at the beginning of the string). After each match, it is set to the index position following the match. Therefore, the output for the preceding code is the following:

0

5

11

18

25

36

44

If the global flag is set to false, the exec() and test() methods do not use or set the lastIndex property.

The match(), replace(), and search() methods of the String class start all searches from the beginning of the string, regardless of the setting of the lastIndex property of the regular expression used in the call to the method. (However, the match() method does set lastIndex to 0.)

You can set the lastIndex property to adjust the starting position in the string for regular expression matching.

The source property

The source property specifies the string that defines the pattern portion of a regular expression. For example:

var pattern:RegExp = /foo/gi; trace(pattern.source); // foo