## Handling errors example: CustomErrors application {#handling-errors-example-customerrors-application}

Flash Player 9 and later, Adobe AIR 1.0 and later

The CustomErrors application demonstrates techniques for working with custom errors when building an application. These techniques are:

*   Validating an XML packet
*   Writing a custom error
*   Throwing custom errors
*   Notifying users when an error is thrown

To get the application files for this sample, see [www.adobe.com/go/learn_programmingAS3samples_flash](http://www.adobe.com/go/learn_programmingAS3samples_flash). The CustomErrors application files can be found in the Samples/CustomError folder. The application consists of the following files:

| **File** | **Description** |
| --- | --- |
| CustomErrors.mxml or | The main application file in Flash (FLA) or Flex (MXML) |
| com/example/programmingas3/errors/ApplicationError.as | A class that serves as the base error class for both the FatalError and WarningError classes. |
| com/example/programmingas3/errors/FatalError.as | A class that defines a FatalError errorthrown by the application. This class extends the custom ApplicationError class. |
| com/example/programmingas3/errors/Validator.as | A class that defines a single method that validates a user-supplied employee XML packet. |
| com/example/programmingas3/errors/WarningError.as | A class that defines a WarningError error thrown by the application. This class extends the custom ApplicationError class. |

**CustomErrors application overview**

Flash Player 9 and later, Adobe AIR 1.0 and later

When the application loads, the initApp() method is called for Flex applications or the timeline (non-function) code is executed for Flash Professional applications. This code defines a sample XML packet to be verified by the Validator class. The following code is run:

employeeXML =

&lt;employee id=&quot;12345&quot;&gt;

&lt;firstName&gt;John&lt;/firstName&gt;

&lt;lastName&gt;Doe&lt;/lastName&gt;

&lt;costCenter&gt;12345&lt;/costCenter&gt;

&lt;costCenter&gt;67890&lt;/costCenter&gt;

&lt;/employee&gt;;

}

The XML packet is later displayed in a TextArea component instance on the Stage. This step allows you to modify the XML packet before attempting to revalidate it.

When the user clicks the Validate button, the validateData() method is called. This method validates the employee XML packet using the validateEmployeeXML() method in the Validator class. The following code shows the validateData() method:

function validateData():void

{

try

{

}

var tempXML:XML = XML(xmlText.text); Validator.validateEmployeeXML(tempXML);

status.text = &quot;The XML was successfully validated.&quot;;

catch (error:FatalError)

{

showFatalError(error);

}

catch (error:WarningError)

{

showWarningError(error);

}

catch (error:Error)

{

showGenericError(error);

}

}

First, a temporary XML object is created using the contents of the TextArea component instance xmlText. Next, the validateEmployeeXML() method in the custom Validator class (com.example.programmingas3/errors/Validator.as) is called and passes the temporary XML object as a parameter. If the XML packet is valid, the status Label component instance displays a success message and the application exits. If the validateEmployeeXML() method throws a custom error (that is, a FatalError, WarningError, or a generic Error occurs), the appropriate catch statement executes and calls either the showFatalError(), showWarningError(), or showGenericError() methods. Each of these methods displays an appropriate message in a text area named statusText to notify the user of the specific error that occurred. Each method also updates the status Label component instance with a specific message.

If a fatal error occurs during an attempt to validate the employee XML packet, the error message is displayed in the statusText text area, and the xmlText TextArea component instance and validateBtn Button component instance are disabled, as the following code shows:

function showFatalError(error:FatalError):void

{

var message:String = error.message + &quot;\n\n&quot;; var title:String = error.getTitle();

statusText.text = message + &quot; &quot; + title + &quot;\n\nThis application has ended.&quot;; this.xmlText.enabled = false;

this.validateBtn.enabled = false; hideButtons();

}

If a warning error instead of a fatal error occurs, the error message is displayed in the statusText TextArea instance, but the xmlText TextField and Button component instances aren’t disabled. The showWarningError() method displays the custom error message in the statusText text area. The message also asks the user to decide if they want to proceed with validating the XML or cancel the script. The following excerpt shows the showWarningError() method:

function showWarningError(error:WarningError):void

{

var message:String = error.message + &quot;\n\n&quot; + &quot;Do you want to exit this application?&quot;; showButtons();

var title:String = error.getTitle(); statusText.text = message;

}

When the user clicks either the Yes or No button, the closeHandler() method is called. The following excerpt shows the closeHandler() method:

function closeHandler(event:CloseEvent):void

{

switch (event.detail)

{

case yesButton:

showFatalError(new FatalError(9999)); break;

case noButton: statusText.text = &quot;&quot;; hideButtons();

break;

}

}

If the user chooses to cancel the script by clicking Yes, a FatalError is thrown, causing the application to terminate.

### Building a custom validator {#building-a-custom-validator}

Flash Player 9 and later, Adobe AIR 1.0 and later

The custom Validator class contains a single method, validateEmployeeXML(). The validateEmployeeXML() method takes a single argument, employee, which is the XML packet that you want to validate. The validateEmployeeXML() method is as follows:

public static function validateEmployeeXML(employee:XML):void

{

// checks for the integrity of items in the XML if (employee.costCenter.length() &lt; 1)

{

throw new FatalError(9000);

}

if (employee.costCenter.length() &gt; 1)

{

throw new WarningError(9001);

}

if (employee.ssn.length() != 1)

{

throw new FatalError(9002);

}

}

To be validated, an employee must belong to one (and only one) cost center. If the employee doesn’t belong to any cost centers, the method throws a FatalError, which bubbles up to the validateData() method in the main application file. If the employee belongs to more than one cost center, a WarningError is thrown. The final check in the XML validator is that the user has exactly one social security number defined (the ssn node in the XML packet). If there is not exactly one ssn node, a FatalError error is thrown.

You can add additional checks to the validateEmployeeXML() method—for example, to ensure that the ssn node contains a valid number, or that the employee has at least one phone number and e-mail address defined, and that both values are valid. You can also modify the XML so that each employee has a unique ID and specifies the ID of their manager.

### Defining the ApplicationError class {#defining-the-applicationerror-class}

Flash Player 9 and later, Adobe AIR 1.0 and later

The ApplicationError class serves as the base class for both the FatalError and WarningError classes. The ApplicationError class extends the Error class, and defines its own custom methods and properties, including defining an error ID, severity, and an XML object that contains the custom error codes and messages. This class also defines two static constants that are used to define the severity of each error type.

The ApplicationError class’s constructor method is as follows:

public function ApplicationError()

{

messages =

&lt;errors&gt;

&lt;error code=&quot;9000&quot;&gt;

&lt;![CDATA[Employee must be assigned to a cost center.]]&gt;

&lt;/error&gt;

&lt;error code=&quot;9001&quot;&gt;

&lt;![CDATA[Employee must be assigned to only one cost center.]]&gt;

&lt;/error&gt;

&lt;error code=&quot;9002&quot;&gt;

&lt;![CDATA[Employee must have one and only one SSN.]]&gt;

&lt;/error&gt;

&lt;error code=&quot;9999&quot;&gt;

&lt;![CDATA[The application has been stopped.]]&gt;

&lt;/error&gt;

&lt;/errors&gt;;

}

Each error node in the XML object contains a unique numeric code and an error message. Error messages can be easily looked up by their error code using E4X, as seen in the following getMessageText() method:

public function getMessageText(id:int):String

{

var message:XMLList = messages.error.(@code == id); return message[0].text();

}

The getMessageText() method takes a single integer argument, id, and returns a string. The id argument is the error code for the error to look up. For example, passing an id of 9001 retrieves the error saying that employees must be assigned to only one cost center. If more than one error has the same error code, ActionScript returns the error message only for the first result found (message[0] in the returned XMLList object).

The next method in this class, getTitle(), doesn’t take any parameters and returns a string value that contains the error ID for this specific error. This value is used to help you easily identify the exact error that occurred during validation of the XML packet. The following excerpt shows the getTitle() method:

public function getTitle():String

{

return &quot;Error #&quot; + id;

}

The final method in the ApplicationError class is toString(). This method overrides the function defined in the Error class so that you can customize the presentation of the error message. The method returns a string that identifies the specific error number and message that occurred.

public override function toString():String

{

return &quot;[APPLICATION ERROR #&quot; + id + &quot;] &quot; + message;

}

### Defining the FatalError class {#defining-the-fatalerror-class}

Flash Player 9 and later, Adobe AIR 1.0 and later

The FatalError class extends the custom ApplicationError class and defines three methods: the FatalError constructor, getTitle(), and toString(). The first method, the FatalError constructor, takes a single integer argument, errorID, and sets the error’s severity using the static constant values defined in the ApplicationError class, and gets the specific error’s error message by calling the getMessageText() method in the ApplicationError class. The FatalError constructor is as follows:

public function FatalError(errorID:int)

{

id = errorID;

severity = ApplicationError.FATAL; message = getMessageText(errorID);

}

The next method in the FatalError class, getTitle(), overrides the getTitle() method defined earlier in the ApplicationError class, and appends the text “-- FATAL” in the title to inform the user that a fatal error has occurred. The getTitle() method is as follows:

public override function getTitle():String

{

return &quot;Error #&quot; + id + &quot; -- FATAL&quot;;

}

The final method in this class, toString(), overrides the toString() method defined in the ApplicationError class.

The toString() method is

public override function toString():String

{

return &quot;[FATAL ERROR #&quot; + id + &quot;] &quot; + message;

}

### Defining the WarningError class {#defining-the-warningerror-class}

Flash Player 9 and later, Adobe AIR 1.0 and later

The WarningError class extends the ApplicationError class and is nearly identical to the FatalError class, except for a couple minor string changes and sets the error severity to ApplicationError.WARNING instead of ApplicationError.FATAL, as seen in the following code:

public function WarningError(errorID:int)

{

id = errorID;

severity = ApplicationError.WARNING; message = super.getMessageText(errorID);

}