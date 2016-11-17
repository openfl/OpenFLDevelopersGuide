# Chapter 54: Printing {#chapter-54-printing}

Flash Player 9 and later, Adobe AIR 1.0 and later

Flash runtimes (such as Adobe® Flash® Player and Adobe® AIR™) can communicate with an operating system’s printing interface so that you can pass pages to the print spooler. Each page sent to the spooler can contain content that is visible, dynamic, or off screen to the user, including database values and dynamic text. Additionally, the properties of the [flash.printing.PrintJob class](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/printing/PrintJob.html) contain values based on a user’s printer settings, so that you can format pages appropriately.

The following content provides strategies for using the flash.printing.PrintJob class methods and properties to create a print job, read a user’s print settings, and adjust a print job based on feedback from a Flash runtime and the user’s operating system.

**Basics of printing**

Flash Player 9 and later, Adobe AIR 1.0 and later

In ActionScript 3.0, you use the PrintJob class to create snapshots of display content to convert to the ink-and-paper representation in a printout. In some ways, setting up content for printing is the same as setting it up for on-screen display—you position and size elements to create the desired layout. However printing has some idiosyncrasies that make it different from screen layout. For example, printers use different resolution than computer monitors; the contents of a computer screen are dynamic and can change, while printed content is inherently static; and in planning printing, consider the constraints of fixed page size and the possibility of multipage printing.

Even though these differences seem obvious, it’s important to keep them in mind when setting up printing with ActionScript. Accurate printing depends on a combination of the values specified by you and the characteristics of the user’s printer. The PrintJob class includes properties that allow you to determine the important characteristics of the user’s printer.

Important concepts and terms

The following reference list contains important terms related to printing:

**Spooler** A portion of the operating system or printer driver software that tracks the pages as they are waiting to be printed and sends them to the printer when it is available.

**Page orientation** The rotation of the printed content in relation to the paper—either horizontal (landscape) or vertical (portrait).

**Print job** The page or set of pages that make up a single printout.