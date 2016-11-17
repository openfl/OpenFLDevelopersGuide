## Strings example: ASCII art {#strings-example-ascii-art}

Flash Player 9 and later, Adobe AIR 1.0 and later

This ASCII Art example shows a number of features of working with the String class in ActionScript 3.0, including the following:

*   The split() method of the String class is used to extract values from a character-delimited string (image information in a tab-delimited text file).
*   Several string-manipulation techniques, including split(), concatenation, and extracting a portion of the string using substring() and substr(), are used to capitalize the first letter of each word in the image titles.
*   The getCharAt() method is used to get a single character from a string (to determine the ASCII character corresponding to a grayscale bitmap value).
*   String concatenation is used to build up the ASCII art representation of an image one character at a time.

The term _ASCII art_ refers to a text representations of an image, in which a grid of monospaced font characters, such as Courier New characters, plots the image. The following image shows an example of ASCII art produced by the application:

![ASCII art - an image rendered with  text characters](C:\Development\Books\HaxeDevelopersGuide\export\assets\ascii_art_-_an_image_rendered_with.jpeg)

_The ASCII art version of the graphic is shown on the right._

To get the application files for this sample, see [www.adobe.com/go/learn_programmingAS3samples_flash](http://www.adobe.com/go/learn_programmingAS3samples_flash). The ASCIIArt application files can be found in the folder Samples/AsciiArt. The application consists of the following files:

| **File** | **Description** |
| --- | --- |
| AsciiArtApp.mxml or | The main application file in Flash (FLA) or Flex (MXML) |
| com/example/programmingas3/asciiArt/AsciiArtBuilder.as | The class that provides the main functionality of the application, including extracting image metadata from a text file, loading the images, and managing the image-to-text conversion process. |
| com/example/programmingas3/asciiArt/BitmapToAsciiConverter.as | A class that provides the parseBitmapData() method for converting image data into a String version. |
| com/example/programmingas3/asciiArt/Image.as | A class which represents a loaded bitmap image. |
| com/example/programmingas3/asciiArt/ImageInfo.as | A class representing metadata for an ASCII art image (such as title, image file URL, and so on). |
| image/ | A folder containing images used by the application. |
| txt/ImageData.txt | A tab-delimited text file, containing information on the images to be loaded by the application. |

**Extracting tab-delimited values**

Flash Player 9 and later, Adobe AIR 1.0 and later

This example uses the common practice of storing application data separate from the application itself; that way, if the data changes (for example, if another image is added or an image’s title changes), there is no need to recreate the SWF file. In this case, the image metadata, including the image title, the URL of the actual image file, and some values that are used to manipulate the image, are stored in a text file (the txt/ImageData.txt file in the project). The contents of the text file are as follows:

FILENAMETITLEWHITE_THRESHHOLDBLACK_THRESHHOLD

FruitBasket.jpgPear, apple, orange, and bananad810 Banana.jpgA picture of a bananaC820 Orange.jpgorangeFF20

Apple.jpgpicture of an apple6E10

The file uses a specific tab-delimited format. The first line (row) is a heading row. The remaining lines contain the following data for each bitmap to be loaded:

*   The filename of the bitmap.
*   The display name of the bitmap.
*   The white-threshold and black-threshold values for the bitmaps. These are hex values above which and below which a pixel is to be considered completely white or completely black.

As soon as the application starts, the AsciiArtBuilder class loads and parses the contents of the text file in order to create the “stack” of images that it will display, using the following code from the AsciiArtBuilder class’s parseImageInfo() method:

var lines:Array = _imageInfoLoader.data.split(&quot;\n&quot;); var numLines:uint = lines.length;

for (var i:uint = 1; i &lt; numLines; i++)

{

var imageInfoRaw:String = lines[i];

...

if (imageInfoRaw.length &gt; 0)

{

// Create a new image info record and add it to the array of image info. var imageInfo:ImageInfo = new ImageInfo();

// Split the current line into values (separated by tab (\t)

// characters) and extract the individual properties: var imageProperties:Array = imageInfoRaw.split(&quot;\t&quot;); imageInfo.fileName = imageProperties[0]; imageInfo.title = normalizeTitle(imageProperties[1]);

imageInfo.whiteThreshold = parseInt(imageProperties[2], 16); imageInfo.blackThreshold = parseInt(imageProperties[3], 16); result.push(imageInfo);

}

}

The entire contents of the text file are contained in a single String instance, the _imageInfoLoader.data property. Using the split() method with the newline character (&quot;\n&quot;) as a parameter, the String instance is divided into an Array (lines) whose elements are the individual lines of the text file. Next, the code uses a loop to work with each of the lines (except the first, because it contains only headers rather than actual content). Inside the loop, the split() method is used once again to divide the contents of the single line into a set of values (the Array object named imageProperties). The parameter used with the split() method in this case is the tab (&quot;\t&quot;) character, because the values in each line are delineated by tab characters.

### Using String methods to normalize image titles {#using-string-methods-to-normalize-image-titles}

Flash Player 9 and later, Adobe AIR 1.0 and later

One of the design decisions for this application is that all the image titles are displayed using a standard format, with the first letter of each word capitalized (except for a few words that are commonly not capitalized in English titles). Rather than assume that the text file contains properly formatted titles, the application formats the titles while they’re being extracted from the text file.

In the previous code listing, as part of extracting individual image metadata values, the following line of code is used:

imageInfo.title = normalizeTitle(imageProperties[1]);

In that code, the image’s title from the text file is passed through the normalizeTitle() method before it is stored in the ImageInfo object:

private function normalizeTitle(title:String):String

{

var words:Array = title.split(&quot; &quot;); var len:uint = words.length;

for (var i:uint; i &lt; len; i++)

{

words[i] = capitalizeFirstLetter(words[i]);

}

return words.join(&quot; &quot;);

}

This method uses the split() method to divide the title into individual words (separated by the space character), passes each word through the capitalizeFirstLetter() method, and then uses the Array class’s join() method to combine the words back into a single string again.

As its name suggests, the capitalizeFirstLetter() method actually does the work of capitalizing the first letter of each word:

/**

*   Capitalizes the first letter of a single word, unless it&#039;s one of
*   a set of words that are normally not capitalized in English.

*/

private function capitalizeFirstLetter(word:String):String

{

switch (word)

{

case &quot;and&quot;: case &quot;the&quot;: case &quot;in&quot;:

case &quot;an&quot;:

case &quot;or&quot;:

case &quot;at&quot;:

case &quot;of&quot;:

case &quot;a&quot;:

// Don&#039;t do anything to these words. break;

default:

// For any other word, capitalize the first character. var firstLetter:String = word.substr(0, 1); firstLetter = firstLetter.toUpperCase();

var otherLetters:String = word.substring(1); word = firstLetter + otherLetters;

}

return word;

}

In English, the initial character of each word in a title is _not_ capitalized if it is one of the following words: “and,” “the,” “in,” “an,” “or,” “at,” “of,” or “a.” (This is a simplified version of the rules.) To execute this logic, the code first uses a switch statement to check if the word is one of the words that should not be capitalized. If so, the code simply jumps out of the switch statement. On the other hand, if the word should be capitalized, that is done in several steps, as follows:

1.  The first letter of the word is extracted using substr(0, 1), which extracts a substring starting with the character at index 0 (the first letter in the string, as indicated by the first parameter 0). The substring will be one character in length (indicated by the second parameter 1).
2.  That character is capitalized using the toUpperCase() method.
3.  The remaining characters of the original word are extracted using substring(1), which extracts a substring starting at index 1 (the second letter) through the end of the string (indicated by leaving off the second parameter of the substring() method).
4.  The final word is created by combining the newly capitalized first letter with the remaining letters using string concatenation: firstLetter + otherLetters.

### Generating the ASCII art text {#generating-the-ascii-art-text}

Flash Player 9 and later, Adobe AIR 1.0 and later

The BitmapToAsciiConverter class provides the functionality of converting a bitmap image to its ASCII text representation. This process is performed by the parseBitmapData() method, which is partially shown here:

var result:String = &quot;&quot;;

// Loop through the rows of pixels top to bottom:

for (var y:uint = 0; y &lt; _data.height; y += verticalResolution)

{

// Within each row, loop through pixels left to right:

for (var x:uint = 0; x &lt; _data.width; x += horizontalResolution)

{

...

// Convert the gray value in the 0-255 range to a value

// in the 0-64 range (since that&#039;s the number of &quot;shades of

// gray&quot; in the set of available characters): index = Math.floor(grayVal / 4);

result += palette.charAt(index);

}

result += &quot;\n&quot;;

}

return result;

This code first defines a String instance named result that will be used to build up the ASCII art version of the bitmap image. Next, it loops through individual pixels of the source bitmap image. Using several color-manipulation techniques (omitted here for brevity), it converts the red, green, and blue color values of an individual pixel to a single grayscale value (a number from 0 to 255). The code then divides that value by 4 (as shown) to convert it to a value in the 0-63 scale, which is stored in the variable index. (The 0-63 scale is used because the “palette” of available ASCII characters used by this application contains 64 values.) The palette of characters is defined as a String instance in the BitmapToAsciiConverter class:

// The characters are in order from darkest to lightest, so that their

// position (index) in the string corresponds to a relative color value

// (0 = black).

private static const palette:String = &quot;@#$%&amp;8BMW*mwqpdbkhaoQ0OZXYUJCLtfjzxnuvcr[]{}1()|/?Il!i&gt;&lt;+_~-;,. &quot;;

Since the index variable defines which ASCII character in the palette corresponds to the current pixel in the bitmap image, that character is retrieved from the palette String using the charAt() method. It is then appended to the result String instance using the concatenation assignment operator (+=). In addition, at the end of each row of pixels, a newline character is concatenated to the end of the result String, forcing the line to wrap to create a new row of character “pixels.”