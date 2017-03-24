# Mouse input example: WordSearch {#mouse-input-example-wordsearch}

This example demonstrates user interaction by handling mouse events. Users build as many words as possible from a random grid of letters, spelling by moving horizontally or vertically in the grid, but never using the same letter twice.This example demonstrates the following features of Haxe:

• Building a grid of components dynamically

• Responding to mouse events

• Maintaining a score based on user interaction

To get the application files for this sample, see [www.adobe.com/go/learn_programmingAS3samples_flash](http://www.adobe.com/go/learn_programmingAS3samples_flash). The WordSearch application files can be found in the folder Samples/WordSearch. The application consists of the following files:

| **File** | **Description** |
| --- | --- |
| WordSearch.as | The class that provides the main functionality of the application. |
| WordSearch.fla or | The main application file for Flex (MXML) or Flash (FLA). |
| dictionary.txt | A file used to determine if spelled words are scorable and spelled correctly. |

**Loading a dictionary**

To create a game that involves finding words, a dictionary is needed. The example includes a text file called dictionary.txt that contains a list of words separated by carriage returns. After creating an array named words, the loadDictionary() function requests this file, and when it loads successfully, the file becomes a long string. You can parse this string into an array of words by using the split() method, breaking at each instance of a carriage return (character code 10) or new line (character code 13). This parsing occurs in the dictionaryLoaded() function:

words = dictionaryText.split(String.fromCharCode(13, 10));

## Creating the user interface {#creating-the-user-interface}

After the words have been stored, you can set up the user interface. Create two Button instances: one for submitting a word, and another for clearing a word that is currently being spelled. In each case, you must respond to user input by listening to the MouseEvent.CLICK event that the button broadcasts and then calling a function. In the setupUI() function, this code creates the listeners on the two buttons:

submitWordButton.addEventListener(MouseEvent.CLICK,submitWord); clearWordButton.addEventListener(MouseEvent.CLICK,clearWord);

## Generating a game board {#generating-a-game-board}

The game board is a grid of random letters. In the generateBoard() function, a two-dimensional grid is created by nesting one loop inside another. The first loop increments rows and the second increments the total number of columns per row. Each of the cells created by these rows and columns contains a button that represents a letter on the board.

private function generateBoard(startX:Number, startY:Number, totalRows:Number, totalCols:Number, buttonSize:Number):void

{

buttons = new Array(); var colCounter:uint; var rowCounter:uint;

for (rowCounter = 0; rowCounter &lt; totalRows; rowCounter++)

{

for (colCounter = 0; colCounter &lt; totalCols; colCounter++)

{

var b:Button = new Button();

b.x = startX + (colCounter*buttonSize);

b.y = startY + (rowCounter*buttonSize); b.addEventListener(MouseEvent.CLICK, letterClicked); b.label = getRandomLetter().toUpperCase(); b.setSize(buttonSize,buttonSize);

b.name = &quot;buttonRow&quot;+rowCounter+&quot;Col&quot;+colCounter; addChild(b);

buttons.push(b);

}

}

}

Although a listener is added for a MouseEvent.CLICK event on only one line, because it is in a for loop, it is assigned to each Button instance. Also, each button is assigned a name derived from its row and column position, which provides an easy way to reference the row and column of each button later in the code.

## Building words from user input {#building-words-from-user-input}

Words can be spelled by selecting letters that are vertically or horizontally adjacent, but never using the same letter twice. Each click generates a mouse event, at which point the word the user is spelling must be checked to ensure it properly continues from letters that have previously been clicked. If it is not, the previous word is removed and a new one is started. This check occurs in the isLegalContinuation() method.

private function isLegalContinuation(prevButton:Button, currButton:Button):Boolean

{

var currButtonRow:Number = Number(currButton.name.charAt(currButton.name. indexOf(&quot;Row&quot;) + 3));

var currButtonCol:Number = Number(currButton.name.charAt(currButton.name.indexOf(&quot;Col&quot;) + 3));

var prevButtonRow:Number = Number(prevButton.name.charAt(prevButton.name.indexOf(&quot;Row&quot;) + 3));

var prevButtonCol:Number = Number(prevButton.name.charAt(prevButton.name.indexOf(&quot;Col&quot;) + 3));

return ((prevButtonCol == currButtonCol &amp;&amp; Math.abs(prevButtonRow - currButtonRow) &lt;= 1) || (prevButtonRow == currButtonRow &amp;&amp; Math.abs(prevButtonCol - currButtonCol) &lt;= 1));

}

The charAt() and indexOf() methods of the String class retrieve the appropriate rows and columns from both the currently clicked button and the previously clicked button. The isLegalContinuation() method returns true if the row or column is unchanged and if the row or column that has been changed is within a single increment from the previous one. If you want to change the rules of the game and allow diagonal spelling, you can remove the checks for an unchanged row or column and the final line would look like this:

return (Math.abs(prevButtonRow - currButtonRow) &lt;= 1) &amp;&amp; Math.abs(prevButtonCol - currButtonCol) &lt;= 1));

## Checking word submissions {#checking-word-submissions}

To complete the code for the game, mechanisms for checking word submissions and tallying the score are needed. The

searchForWord() method contains both:

private function searchForWord(str:String):Number

{

if (words &amp;&amp; str)

{

var i:uint = 0

for (i = 0; i &lt; words.length; i++)

{

var thisWord:String = words[i]; if (str == words[i])

{

return i;

}

}

return -1;

}

else

{

trace(&quot;WARNING: cannot find words, or string supplied is null&quot;);

}

return -1;

}

This function loops through all of the words in the dictionary. If the user’s word matches a word in the dictionary, its position in the dictionary is returned. The submitWord() method then checks the response and updates the score if the position is valid.

## Customization {#customization}

At the beginning of the class are several constants. You can modify this game by modifying these variables. For example, you can change the amount of time available to play by increasing the TOTAL_TIME variable. You can also increase the PERCENT_VOWELS variable slightly to increase the likelihood of finding words.