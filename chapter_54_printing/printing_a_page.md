## Printing a page {#printing-a-page}

Flash Player 9 and later, Adobe AIR 1.0 and later

You use an instance of the PrintJob class to handle printing. To print a basic page through Flash Player or AIR, you use these four statements in sequence:

• new PrintJob(): Creates a new print job instance of the name you specify.

• PrintJob.start(): Initiates the printing process for the operating system, calling the print dialog box for the user, and populates the read-only properties of the print job.

• PrintJob.addPage(): Contains the details about the print job contents, including the Sprite object (and any children it contains), the size of the print area, and whether the printer prints the image as a vector or bitmap. You can use successive calls to addPage() to print multiple sprites over several pages.

• PrintJob.send(): Sends the pages to the operating system’s printer.

So, for example, a simple print job script is (including package, import and class statements for compiling):

package

{

import flash.printing.PrintJob; import flash.display.Sprite;

public class BasicPrintExample extends Sprite

{

var myPrintJob:PrintJob = new PrintJob(); var mySprite:Sprite = new Sprite();

public function BasicPrintExample()

{

myPrintJob.start(); myPrintJob.addPage(mySprite); myPrintJob.send();

}

}

}

**_Note:_ **_This example is intended to show the basic elements of a print job script, and does not contain any error handling. To build a script that responds properly to a user canceling a print job, see_

_“Working with exceptions and returns” on_

_page 926_

_._

To clear a PrintJob object’s properties for any reason, set the PrintJob variable to null (as in myPrintJob = null).