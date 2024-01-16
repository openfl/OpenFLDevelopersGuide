# TextField Example: Newspaper-style text formatting {#textfield-example-newspaper-style-text-formatting}

The News Layout example formats text to look something like a story in a printed newspaper. The input text can contain a headline, a subtitle, and the body of the story. Given a display width and height, this News Layout example formats the headline and the subtitle to take the full width of the display area. The story text is distributed across two or more columns.

This example illustrates the following Haxe programming techniques:

*   Extending the TextField class
*   Loading and applying an external CSS file
*   Converting CSS styles into TextFormat objects
*   Using the TextLineMetrics class to get information about text display size

To get the application files for this sample, see [www.adobe.com/go/learn_programmingAS3samples_flash](http://www.adobe.com/go/learn_programmingAS3samples_flash). The News Layout application files can be found in the folder Samples/NewsLayout. The application consists of the following files:

| **File** | **Description** |
| --- | --- |
| NewsLayout.mxml or | The user interface for the application for Flex (MXML) or Flash (FLA). |
| com/example/programmingas3/ne wslayout/StoryLayoutComponent.a s | A Flex UIComponent class that places the StoryLayout instance. |
| com/example/programmingas3/ne wslayout/StoryLayout.as | The main Haxe class that arranges all the components of a news story for display. |
| com/example/programmingas3/ne wslayout/FormattedTextField.as | A subclass of the TextField class that manages its own TextFormat object. |
| com/example/programmingas3/ne wslayout/HeadlineTextField.as | A subclass of the FormattedTextField class that adjusts font sizes to fit a desired width. |
| com/example/programmingas3/ne wslayout/MultiColumnTextField.as | An Haxe class that splits text across two or more columns. |
| story.css | A CSS file that defines text styles for the layout. |

**Reading the external CSS file**

The News Layout application starts by reading story text from a local XML file. Then it reads an external CSS file that provides the formatting information for the headline, subtitle, and main text.

The CSS file defines three styles, a standard paragraph style for the story, and the h1 and h2 styles for the headline and subtitle respectively.

p {

font-family: Georgia, "Times New Roman", Times, _serif; font-size: 12;

leading: 2;

text-align: justify; indent: 24;

}

h1 {

font-family: Verdana, Arial, Helvetica, _sans; font-size: 20;

font-weight: bold; color: #000099; text-align: left;

}

h2 {

font-family: Verdana, Arial, Helvetica, _sans; font-size: 16;

font-weight: normal; text-align: left;

}

The technique used to read the external CSS file is the same as the technique described in

"Loading an external CSS

file" on page 381

. When the CSS file has been loaded the application executes the onCSSFileLoaded() method, shown below.

public function onCSSFileLoaded(event:Event):void

{

this.sheet = new StyleSheet(); this.sheet.parseCSS(loader.data);

h1Format = getTextStyle("h1", this.sheet); if (h1Format == null)

{

h1Format = getDefaultHeadFormat();

}

h2Format = getTextStyle("h2", this.sheet); if (h2Format == null)

{

h2Format = getDefaultHeadFormat(); h2Format.size = 16;

}

pFormat = getTextStyle("p", this.sheet); if (pFormat == null)

{

pFormat = getDefaultTextFormat(); pFormat.size = 12;

}

displayText();

}

The onCSSFileLoaded() method creates a StyleSheet object and has it parse the input CSS data. The main text for the story is displayed in a MultiColumnTextField object, which can use a StyleSheet object directly. However, the headline fields use the HeadlineTextField class, which uses a TextFormat object for its formatting.

The onCSSFileLoaded() method calls the getTextStyle() method twice to convert a CSS style declaration into a TextFormat object for use with each of the two HeadlineTextField objects.

public function getTextStyle(styleName:String, ss:StyleSheet):TextFormat

{

var format:TextFormat = null;

var style:Object = ss.getStyle(styleName); if (style != null)

{

var colorStr:String = style.color;

if (colorStr != null &amp;&amp; colorStr.indexOf("#") == 0)

{

style.color = colorStr.substr(1);

}

format = new TextFormat(style.fontFamily,

style.fontSize, style.color,

(style.fontWeight == "bold"), (style.fontStyle == "italic"), (style.textDecoration == "underline"), style.url,

style.target, style.textAlign, style.marginLeft, style.marginRight, style.indent, style.leading);

if (style.hasOwnProperty("letterSpacing"))

{

format.letterSpacing = style.letterSpacing;

}

}

return format;

}

The property names and the meaning of the property values differ between CSS style declarations and TextFormat objects. The getTextStyle() method translates CSS property values into the values expected by the TextFormat object.

## Arranging story elements on the page {#arranging-story-elements-on-the-page}

The StoryLayout class formats and lays out the headline, subtitle, and main text fields into a newspaper-style arrangement. The displayText() method initially creates and places the various fields.

public function displayText():void

{

headlineTxt = new HeadlineTextField(h1Format); headlineTxt.wordWrap = true;

headlineTxt.x = this.paddingLeft; headlineTxt.y = this.paddingTop; headlineTxt.width = this.preferredWidth; this.addChild(headlineTxt);

headlineTxt.fitText(this.headline, 1, true);

subtitleTxt = new HeadlineTextField(h2Format); subtitleTxt.wordWrap = true;

subtitleTxt.x = this.paddingLeft;

subtitleTxt.y = headlineTxt.y + headlineTxt.height; subtitleTxt.width = this.preferredWidth; this.addChild(subtitleTxt);

subtitleTxt.fitText(this.subtitle, 2, false); storyTxt = new MultiColumnText(this.numColumns, 20,

this.preferredWidth, 400, true, this.pFormat); storyTxt.x = this.paddingLeft;

storyTxt.y = subtitleTxt.y + subtitleTxt.height + 10; this.addChild(storyTxt);

storyTxt.text = this.content;

...

Each field is placed below the previous field by setting its y property to equal the y property of the previous field plus its height. This dynamic placement calculation is needed because HeadlineTextField objects and MultiColumnTextField objects can change their height to fit their contents.

## Altering font size to fit the field size {#altering-font-size-to-fit-the-field-size}

Given a width in pixels and a maximum number of lines to display, the HeadlineTextField alters the font size to make the text fit the field. If the text is short, the font size is large, creating a tabloid-style headline. If the text is long, the font size is smaller.

The HeadlineTextField.fitText() method shown below does the font sizing work:

public function fitText(msg:String, maxLines:uint = 1, toUpper:Boolean = false, targetWidth:Number = -1):uint

{

this.text = toUpper ? msg.toUpperCase() : msg;

if (targetWidth == -1)

{

targetWidth = this.width;

}

var pixelsPerChar:Number = targetWidth / msg.length;

var pointSize:Number = Math.min(MAX_POINT_SIZE, Math.round(pixelsPerChar * 1.8 * maxLines)); if (pointSize &lt; 6)

{

// the point size is too small return pointSize;

}

this.changeSize(pointSize);

if (this.numLines &gt; maxLines)

{

}

else

{

}

}

return shrinkText(--pointSize, maxLines);

return growText(pointSize, maxLines);

public function growText(pointSize:Number, maxLines:uint = 1):Number

{

if (pointSize &gt;= MAX_POINT_SIZE)

{

return pointSize;

}

this.changeSize(pointSize + 1);

if (this.numLines &gt; maxLines)

{

}

else

{

}

// set it back to the last size this.changeSize(pointSize); return pointSize;

return growText(pointSize + 1, maxLines);

}

public function shrinkText(pointSize:Number, maxLines:uint=1):Number

{

if (pointSize &lt;= MIN_POINT_SIZE)

{

return pointSize;

}

this.changeSize(pointSize);

if (this.numLines &gt; maxLines)

{

}

else

{

}

}

return shrinkText(pointSize - 1, maxLines);

return pointSize;

The HeadlineTextField.fitText() method uses a simple recursive technique to size the font. First it guesses an average number of pixels per character in the text and from there calculates a starting point size. Then it changes the font size and checks whether the text has word wrapped to create more than the maximum number of text lines. If there are too many lines it calls the shrinkText() method to decrease the font size and try again. If there are not too many lines it calls the growText() method to increase the font size and try again. The process stops at the point where incrementing the font size by one more point would create too many lines.

## Splitting text across multiple columns {#splitting-text-across-multiple-columns}

The MultiColumnTextField class spreads text among multiple TextField objects which are then arranged like newspaper columns.

The MultiColumnTextField() constructor first creates an array of TextField objects, one for each column, as shown here:

for (var i:int = 0; i &lt; cols; i++)

{

var field:TextField = new TextField(); field.multiline = true;

field.autoSize = TextFieldAutoSize.NONE; field.wordWrap = true;

field.width = this.colWidth; field.setTextFormat(this.format); this.fieldArray.push(field); this.addChild(field);

}

Each TextField object is added to the array and added to the display list with the addChild() method.

Whenever the StoryLayout text property or styleSheet property changes, it calls the layoutColumns() method to redisplay the text. The layoutColumns() method calls the getOptimalHeight() method, to figure out the correct pixel height that is needed to fit all of the text within the given layout width.

public function getOptimalHeight(str:String):int

{

if (field.text == "" || field.text == null)

{

return this.preferredHeight;

}

else

{

this.linesPerCol = Math.ceil(field.numLines / this.numColumns);

var metrics:TextLineMetrics = field.getLineMetrics(0); this.lineHeight = metrics.height;

var prefHeight:int = linesPerCol * this.lineHeight;

return prefHeight + 4;

}

}

First the getOptimalHeight() method calculates the width of each column. Then it sets the width and htmlText property of the first TextField object in the array. The getOptimalHeight() method uses that first TextField object to discover the total number of word-wrapped lines in the text, and from that it identifies how many lines should be in each column. Next it calls the TextField.getLineMetrics() method to retrieve a TextLineMetrics object that contains details about size of the text in the first line. The TextLineMetrics.height property represents the full height of a line of text, in pixels, including the ascent, descent, and leading. The optimal height for the MultiColumnTextField object is then the line height multiplied by the number of lines per column, plus 4 to account for the two-pixel border at the top and the bottom of a TextField object.

Here is the code for the full layoutColumns() method:

public function layoutColumns():void

{

if (this._text == "" || this._text == null)

{

return;

}

var field:TextField = fieldArray[0] as TextField; field.text = this._text; field.setTextFormat(this.format);

this.preferredHeight = this.getOptimalHeight(field); var remainder:String = this._text;

var fieldText:String = "";

var lastLineEndedPara:Boolean = true;

var indent:Number = this.format.indent as Number; for (var i:int = 0; i &lt; fieldArray.length; i++)

{

field = this.fieldArray[i] as TextField;

field.height = this.preferredHeight; field.text = remainder;

field.setTextFormat(this.format);

var lineLen:int;

if (indent &gt; 0 &amp;&amp; !lastLineEndedPara &amp;&amp; field.numLines &gt; 0)

{

lineLen = field.getLineLength(0); if (lineLen &gt; 0)

{

field.setTextFormat(this.firstLineFormat, 0, lineLen);

}

}

field.x = i * (colWidth + gutter); field.y = 0;

remainder = ""; fieldText = "";

var linesRemaining:int = field.numLines;

var linesVisible:int = Math.min(this.linesPerCol, linesRemaining);

for (var j:int = 0; j &lt; linesRemaining; j++)

{

if (j &lt; linesVisible)

{

fieldText += field.getLineText(j);

}

else

{

remainder +=field.getLineText(j);

}

}

field.text = fieldText; field.setTextFormat(this.format);

if (indent &gt; 0 &amp;&amp; !lastLineEndedPara)

{

lineLen = field.getLineLength(0); if (lineLen &gt; 0)

{

field.setTextFormat(this.firstLineFormat, 0, lineLen);

}

}

var lastLine:String = field.getLineText(field.numLines - 1);

var lastCharCode:Number = lastLine.charCodeAt(lastLine.length - 1);

if (lastCharCode == 10 || lastCharCode == 13)

{

lastLineEndedPara = true;

}

else

{

lastLineEndedPara = false;

}

if ((this.format.align == TextFormatAlign.JUSTIFY) &amp;&amp; (i &lt; fieldArray.length - 1))

{

if (!lastLineEndedPara)

{

justifyLastLine(field, lastLine);

}

}

}

}

After the preferredHeight property has been set by calling the getOptimalHeight() method, the layoutColumns() method iterates through the TextField objects, setting the height of each to the preferredHeight value. The layoutColumns() method then distributes just enough lines of text to each field so that no scrolling occurs in any individual field, and the text in each successive field begins where the text in the previous field ended. If the text alignment style has been set to "justify" then the justifyLastLine() method is called to justify the final line of text in a field. Otherwise that last line would be treated as an end-of-paragraph line and not justified.