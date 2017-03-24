# Chapter 47: Using the external API {#chapter-47-using-the-external-api}

The Haxe external API ([flash.external.ExternalInterface](http://help.adobe.com/en_US/FlashPlatform/reference/Haxe/3/flash/external/ExternalInterface.html)) enables straightforward communication between Haxe and the container application within which Adobe OpenFL is running. Use the ExternalInterface API to create interaction between a SWF document and JavaScript in an HTML page.

You can use the external API to interact with a container application, pass data between Haxe and JavaScript in an HTML page.

Some common external API tasks are:

*   Getting information about the container application
*   Using Haxe to call code in a web page displayed in a browser or an AIR desktop application
*   Calling Haxe code from a web page
*   Creating a proxy to simplify calling Haxe code from a web page

**_Note:_ **_This discussion of the external interface only covers communication between Haxe in a project and the container application that includes a reference to the OpenFL or instance in which the project is loaded. Any other use of OpenFL within an application is outside the scope of this documentation. OpenFL is designed to be used as a browser plug-in or as a projector (standalone application). Other usage scenarios may have limited support._

Using the external API in AIR

Since an AIR application does not have an external container, this external interface does not generally apply—nor is it generally needed. When your AIR application loads a project directly, the application code can communicate directly with the Haxe code in the SWF (subject to security sandbox restrictions).

However, when your AIR application loads a project using an HTML page in an HTMLLoader object (or an HTML component in Flex), the HTMLLoader object serves as the external container. Thus, you can use the external interface to communicate between the Haxe code in the loaded SWF and the JavaScript code in the HTML page loaded in the HTMLLoader.

**Basics of using the external API**

Although in some cases a project can run on its own (for example, if you use Adobe® Flash® Professional to create a SWF projector), in the majority of cases a SWF application runs as an element inside of another application.

Commonly, the container that includes the SWF is an HTML file; somewhat less frequently, a project is used for all or part of the user interface of a desktop application.

As you work on more advanced applications, you may find a need to set up communication between the project and the container application. For instance, it’s common for a web page to display text or other information in HTML, and include a project to display dynamic visual content such as a chart or video. In such a case, you might want to make it so that when users click a button on the web page, it changes something in the project. Haxe contains a mechanism, known as the external API, that facilitates this type of communication between Haxe in a project and other code in the container application.

Important concepts and terms

The following reference list contains important terms relevant to this feature:

**Container application** The application within which OpenFL is running a project, such as a web browser and HTML page that includes OpenFL content or an AIR application that loads the SWF in a web page..

**Projector** An executable file that includes SWF content and an embedded version of OpenFL. You can create a projector file using Flash Professional or the standalone OpenFL. Projectors are commonly used to distribute projects by CD-ROM or in similar situations where download size is not an issue and the SWF author wants to be certain the user will be able to run the project, regardless of whether OpenFL is installed on the user’s computer.

**Proxy** A go-between application or code that calls code in one application (the “external application”) on behalf of another application (the “calling application”), and returns values to the calling application. A proxy can be used for various reasons, including:

*   To simplify the process of making external function calls by converting native function calls in the calling application into the format understood by the external application.
*   To work around security or other restrictions that prevent the caller from communicating directly with the external application.

**Serialize** To convert objects or data values into a format that can be used to pass the values in messages between two programming systems, such as over the Internet or between two different applications running on a single computer.

Working through the examples

Many of the code examples provided are small listings of code for demonstration purposes rather than full working examples or code that checks values. Because using the external API requires (by definition) writing Haxe code as well as code in a container application, testing the examples involves creating a container (for example, a web page containing the project) and using the code listings to interact with the container.

To test an example of Haxe-to-JavaScript communication:

1.  Create a new document using Flash Professional and save it to your computer.
2.  From the main menu, choose File &gt; Publish Settings.
3.  In the Publish Settings dialog box, on the Formats tab, confirm that the Flash and HTML check boxes are selected.
4.  Click the Publish button. This generates a project and HTML file in the same folder and with the same name that you used to save the document. Click OK to close the Publish Settings dialog box.
5.  Deselect the HTML check box. Now that the HTML page is generated, you are going to modify it to add the appropriate JavaScript code. Deselecting the HTML check box ensures that after you modify the HTML page, Flash will not overwrite your changes with a new HTML page when it’s publishing the project.
6.  Click OK to close the Publish Settings dialog box.
7.  With an HTML or text editor application, open the HTML file that was created by Flash when you published the project. In the HTML source code, add opening and closing script tags, and copy into them the JavaScript code from the example code listing:

&lt;script&gt;

// add the sample JavaScript code here

&lt;/script&gt;

1.  Save the HTML file and return to Flash.
2.  Select the keyframe on Frame 1 of the Timeline, and open the Actions panel.
3.  Copy the Haxe code listing into the Script pane.
4.  From the main menu, choose File &gt; Publish to update the project with the changes that you’ve made.
5.  Using a web browser, open the HTML page you edited to view the page and test communication between Haxe and the HTML page.

To test an example of Haxe-to-ActiveX container communication:

1.  Create a new document using Flash Professional and save it to your computer. You may want to save it in the folder where your container application will expect to find the project.
2.  From the main menu, choose File &gt; Publish Settings.
3.  In the Publish Settings dialog box, on the Formats tab, confirm that only the Flash check box is selected.
4.  In the File field next to the Flash check box, click the folder icon to select the folder into which your project will be published. By setting the location for your project, you can (for example) keep the document in one folder, but put the published project in another folder such as the folder containing the source code for the container application.
5.  Select the keyframe on Frame 1 of the Timeline, and open the Actions panel.
6.  Copy the Haxe code for the example into the Script pane.
7.  From the main menu, choose File &gt; Publish to re-publish the project.
8.  Create and run your container application to test communication between Haxe and the container application.

For full examples of using the external API to communicate with an HTML page, see the following topic:

*   “External API example: Communicating between Haxe and JavaScript in a web browser” on page 849

Those examples include the full code, including Haxe and container error-checking code, which you should use when writing code using the external API. For another full example using the external API, see the class example for the ExternalInterface class in the Haxe Reference.