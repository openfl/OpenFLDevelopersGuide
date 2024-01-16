# Chapter 39: Storing local data {#chapter-39-storing-local-data}

You use the [SharedObject](https://api.openfl.org/openfl/net/SharedObject.html) class to store small amounts of data on the client computer. In Adobe AIR, you can also use the [EncryptedLocalStore](https://api.openfl.org/openfl/data/EncryptedLocalStore.html) class to store small amounts of privacy-sensitive user data on the local computer in an AIR application.

You can also read and write files on the file system and (in Adobe AIR) access local database files. For more information, see

"Working with the file system" on page 653

and

"Working with local SQL databases in AIR" on

page 714

.

There are a number of security factors that relate to shared objects. For more information, see

"Shared objects" on

page 1074

in

"Security" on page 1042

.

**Shared objects**

A shared object, sometimes referred to as a "Flash cookie," is a data file that can be created on your computer by the sites that you visit. Shared objects are most often used to enhance your web-browsing experience—for example, by allowing you to personalize the look and feel of a website that you frequently visit.

**About shared objects**

Shared objects function like browser cookies. You use the [SharedObject](https://api.openfl.org/openfl/net/SharedObject.html) class to store data on the user’s local hard disk and call that data during the same session or in a later session. Applications can access only their own SharedObject data, and only if they are running on the same domain. The data is not sent to the server and is not accessible by other applications running on other domains, but can be made accessible by applications from the same domain.

**Shared objects compared with cookies**

Cookies and shared objects are very similar. Because most web programmers are familiar with how cookies work, it might be useful to compare cookies and local SharedObjects.

Cookies that adhere to the RFC 2109 standard generally have the following properties:

*   They can expire, and often do at the end of a session by default.
*   They can be disabled by the client on a site-specific basis.
*   There is a limit of 300 cookies total, and 20 cookies maximum per site.
*   They are usually limited to a size of 4 KB each.
*   They are sometimes perceived to be a security threat, and as a result, they are sometimes disabled on the client.
*   They are stored in a location specified by the client browser.
*   They are transmitted from client to server through HTTP. In contrast, shared objects have the following properties:
*   They do not expire by default.
*   By default, they are limited to a size of 100 KB each.
*   They can store simple data types (such as String, Array, and Date).
*   They are stored in a location specified by the application (within the user’s home directory).
*   They are never transmitted between the client and server.

About the SharedObject class

Using the [SharedObject](https://api.openfl.org/openfl/net/SharedObject.html) class, you can create and delete shared objects, as well as detect the current size of a SharedObject object that you are using.

## Creating a shared object {#creating-a-shared-object}

To create a [SharedObject](https://api.openfl.org/openfl/net/SharedObject.html) object, use the SharedObject.getLocal() method, which has the following syntax:

SharedObject.getLocal("objectName" [, pathname]): SharedObject

The following example creates a shared object called mySO:

public var mySO:SharedObject;

mySO = SharedObject.getLocal("preferences");

This creates a file on the client’s machine called preferences.sol.

The term _local_ refers to the location of the shared object. In this case, Adobe® Flash® Player stores the SharedObject file locally in the client’s home directory.

When you create a shared object, OpenFL creates a new directory for the application and domain inside its sandbox. It also creates a *.sol file that stores the SharedObject data. The default location of this file is a subdirectory of the user’s home directory. The following table shows the default locations of this directory:

| **Operating System** | **Location** |
| --- | --- |
| Windows 95/98/ME/2000/XP | c:/Documents and Settings/_username_/Application Data/Macromedia/OpenFL/#SharedObjects |
| Windows Vista/Windows 7 | c:/Users/_username_/AppData/Roaming/Macromedia/OpenFL/#SharedObjects |
| Macintosh OS X | /Users/_username_/Library/Preferences/Macromedia/OpenFL/#SharedObjects/_web_domain_/_path_to_application_/_applicatio n_name_/_object_n_ame.sol |
| Linux/Unix | /home/username/.macromedia/Flash_Player/#SharedObjects/_web_doma in_/_path_to_appl_ication/_application_name_/_object_name_.sol |

Below the #SharedObjects directory is a randomly-named directory. Below that is a directory that matches the hostname, then the path to the application, and finally the *.sol file.

For example, if you request an application named MyApp.swf on the local host, and within a subdirectory named /sos, OpenFL stores the *.sol file in the following location on Windows XP:

c:/Documents and Settings/fred/Application Data/Macromedia/OpenFL/#SharedObjects/KROKWXRK/#localhost/sos/MyApp.swf/data.sol

**_Note:_** _If you do not provide a name in the SharedObject.getLocal() method, OpenFL names the file undefined.sol._

By default, Flash can save locally persistent SharedObject objects of up to 100 KB per domain. This value is user- configurable. When the application tries to save data to a shared object that would make it bigger than 100 KB, OpenFL displays the Local Storage dialog box, which lets the user allow or deny more local storage for the domain that is requesting access.

**Specifying a path**

You can use the optional _pathname_ parameter to specify a location for the [SharedObject](https://api.openfl.org/openfl/net/SharedObject.html) file. This file must be a subdirectory of that domain’s SharedObject directory. For example, if you request an application on the localhost and specify the following:

mySO = SharedObject.getLocal("myObjectFile","/");

OpenFL writes the SharedObject file in the /#localhost directory (or /localhost if the application is offline). This is useful if you want more than one application on the client to be able to access the same shared object. In this case, the client could run two Flex applications, both of which specify a path to the shared object that is the root of the domain; the client could then access the same shared object from both applications. To share data between more than application without persistence, you can use the LocalConnection object.

If you specify a directory that does not exist, OpenFL does not create a SharedObject file.

Adding data to a shared object

You add data to a [SharedObject](https://api.openfl.org/openfl/net/SharedObject.html)’s *.sol file using the data property of the SharedObject object. To add new data to the shared object, use the following syntax:

_sharedObject_name_.data._variable_ = _value_;

The following example adds the userName, itemNumbers, and adminPrivileges properties and their values to a SharedObject:

public var currentUserName:String = "Reiner";

public var itemsArray:Array = new Array(101,346,483);
public var currentUserIsAdmin:Boolean = true;
mySO.data.userName = currentUserName;
mySO.data.itemNumbers = itemsArray;
mySO.data.adminPrivileges = currentUserIsAdmin;

After you assign values to the data property, you must instruct OpenFL to write those values to the SharedObject’s file. To force OpenFL to write the values to the SharedObject’s file, use the SharedObject.flush() method, as follows:

mySO.flush();

If you do not call the SharedObject.flush() method, OpenFL writes the values to the file when the application quits. However, this does not provide the user with an opportunity to increase the available space that OpenFL has to store the data if that data exceeds the default settings. Therefore, it is a good practice to call SharedObject.flush().

When using the flush() method to write shared objects to a user’s hard drive, you should be careful to check whether the user has explicitly disabled local storage using the OpenFL Settings Manager ([www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html)), as shown in the following example:

var so:SharedObject = SharedObject.getLocal("test"); trace("Current SharedObject size is " + so.size + " bytes."); so.flush();

Storing objects in shared objects

You can store simple objects such as Arrays or Strings in a SharedObject’s data property.

The following example is an Haxe class that defines methods that control the interaction with the shared object. These methods let the user add and remove objects from the shared object. This class stores an ArrayCollection that contains simple objects.

package {

import mx.collections.ArrayCollection;
import openfl.net.SharedObject;

public class LSOHandler {

private var mySO:SharedObject;
private var ac:ArrayCollection;
private var lsoType:String;

// The parameter is "feeds" or "sites". public function LSOHandler(s:String) {

init(s);

}

private function init(s:String):void {
	ac = new ArrayCollection();
	lsoType = s;

mySO = SharedObject.getLocal(lsoType);
if (getObjects()) {

ac = getObjects();

}

}

public function getObjects():ArrayCollection {
	return mySO.data[lsoType];

}

public function addObject(o:Object):void {
	ac.addItem(o);

updateSharedObjects();

}

private function updateSharedObjects():void {
	mySO.data[lsoType] = ac;

mySO.flush();

}

}

}

The following Flex application creates an instance of the Haxe class for each of the types of shared objects it needs. It then calls methods on that class when the user adds or removes blogs or site URLs.

&lt;?xml version="1.0"?&gt;

&lt;!-- lsos/BlogAggregator.mxml --&gt;

&lt;mx:Application xmlns:local="*"

[xmlns:mx="http://www.adobe.com/2006/mxml"](http://www.adobe.com/2006/mxml) creationComplete="initApp()" backgroundColor="#ffffff"

&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import mx.collections.ArrayCollection;
import mx.utils.ObjectUtil;

import openfl.net.SharedObject;

[Bindable]

public var welcomeMessage:String;

[Bindable]

public var localFeeds:ArrayCollection = new ArrayCollection();

[Bindable]

public var localSites:ArrayCollection = new ArrayCollection();

public var lsofeeds:LSOHandler;
public var lsosites:LSOHandler;

private function initApp():void {
	lsofeeds = new LSOHandler("feeds");
	lsosites = new LSOHandler("sites");

if (lsofeeds.getObjects()) {

localFeeds = lsofeeds.getObjects();

}

if (lsosites.getObjects()) {

localSites = lsosites.getObjects();

}

}

// Adds a new feed to the feeds DataGrid.
private function addFeed():void {

// Construct an object you want to store in the

// LSO. This object can contain any number of fields.

var o:Object = {name:ti1.text, url:ti2.text, date:new Date()};
lsofeeds.addObject(o);

// Because the DataGrid's dataProvider property is

// bound to the ArrayCollection, Flex updates the

// DataGrid when you call this method.
localFeeds = lsofeeds.getObjects();

// Clear the text fields.
ti1.text = '';

ti2.text = '';

}

// Removes feeds from the feeds DataGrid.
private function removeFeed():void {

// Use a method of ArrayCollection to remove a feed.

// Because the DataGrid's dataProvider property is

// bound to the ArrayCollection, Flex updates the

// DataGrid when you call this method. You do not need

// to update it manually.

if (myFeedsGrid.selectedIndex &gt; -1) {

localFeeds.removeItemAt(myFeedsGrid.selectedIndex);

}

}

private function addSite():void {

var o:Object = {name:ti3.text, date:new Date()};
lsosites.addObject(o);

localSites = lsosites.getObjects(); ti3.text = '';

}

private function removeSite():void {

if (mySitesGrid.selectedIndex &gt; -1) {

localSites.removeItemAt(mySitesGrid.selectedIndex);

}

}

]]&gt;

&lt;/mx:Script&gt;

&lt;mx:Label text="Blog aggregator" fontSize="28"/&gt;

&lt;mx:Panel title="Blogs"&gt;

&lt;mx:Form id="blogForm"&gt;

&lt;mx:HBox&gt;

&lt;mx:FormItem label="Name:"&gt;

&lt;mx:TextInput id="ti1" width="100"/&gt;

&lt;/mx:FormItem&gt;

&lt;mx:FormItem label="Location:"&gt;

&lt;mx:TextInput id="ti2" width="300"/&gt;

&lt;/mx:FormItem&gt;

&lt;mx:Button id="b1" label="Add Feed" click="addFeed()"/&gt;

&lt;/mx:HBox&gt;

&lt;mx:FormItem label="Existing Feeds:"&gt;

&lt;mx:DataGrid

id="myFeedsGrid" dataProvider="{localFeeds}" width="400"

/&gt;

&lt;/mx:FormItem&gt;

&lt;mx:Button id="b2" label="Remove Feed" click="removeFeed()"/&gt;

&lt;/mx:Form&gt;

&lt;/mx:Panel&gt;

&lt;mx:Panel title="Sites"&gt;

&lt;mx:Form id="siteForm"&gt;

&lt;mx:HBox&gt;

&lt;mx:FormItem label="Site:"&gt;

&lt;mx:TextInput id="ti3" width="400"/&gt;

&lt;/mx:FormItem&gt;

&lt;mx:Button id="b3" label="Add Site" click="addSite()"/&gt;

&lt;/mx:HBox&gt;

&lt;mx:FormItem label="Existing Sites:"&gt;

&lt;mx:DataGrid

id="mySitesGrid" dataProvider="{localSites}" width="400"

/&gt;

&lt;/mx:FormItem&gt;

&lt;mx:Button id="b4" label="Remove Site" click="removeSite()"/&gt;

&lt;/mx:Form&gt;

&lt;/mx:Panel&gt;

&lt;/mx:Application&gt;

Storing typed objects in shared objects

You can store typed Haxe instances in shared objects. You do this by calling the openfl.net.registerClassAlias() method to register the class. If you create an instance of your class and store it in the data member of your shared object and later read the object out, you will get a typed instance. By default, the SharedObject objectEncoding property supports AMF3 encoding, and unpacks your stored instance from the SharedObject object; the stored instance retains the same type you specified when you called the registerClassAlias() method.

(iOS only) Prevent cloud backup of local shared objects

Adobe AIR 3.7 and later, iOS only

You can set the SharedObject.preventBackup property to control whether local shared objects will be backed up on the iOS cloud backup service. This is required by Apple for content that can be regenerated or downloaded again, but that is required for proper functioning of your application during offline use.

Creating multiple shared objects

You can create multiple shared objects for the same Flex application. To do this, you assign each of them a different instance name, as the following example shows:

public var mySO:SharedObject = SharedObject.getLocal("preferences");
public var mySO2:SharedObject = SharedObject.getLocal("history");

This creates a preferences.sol file and a history.sol file in the Flex application’s local directory.

## Creating a secure SharedObject {#creating-a-secure-sharedobject}

When you create either a local or remote SharedObject using getLocal() or getRemote(), there is an optional parameter named secure that determines whether access to this shared object is restricted to projects that are delivered over an HTTPS connection. If this parameter is set to true and your project is delivered over HTTPS, OpenFL creates a new secure shared object or gets a reference to an existing secure shared object. This secure shared object can be read from or written to only by projects delivered over HTTPS that call SharedObject.getLocal() with the secure parameter set to true. If this parameter is set to false and your project is delivered over HTTPS, OpenFL creates a new shared object or gets a reference to an existing shared object.

This shared object can be read from or written to by projects delivered over non-HTTPS connections. If your project is delivered over a non-HTTPS connection and you try to set this parameter to true, the creation of a new shared object (or the access of a previously created secure shared object) fails, an error is thrown, and the shared object is set to null. If you attempt to run the following snippet from a non-HTTPS connection, the SharedObject.getLocal() method will throw an error:

try

{

var so:SharedObject = SharedObject.getLocal("contactManager", null, true);

}

catch (error:Error)

{

trace("Unable to create SharedObject.");

}

Regardless of the value of this parameter, the created shared objects count toward the total amount of disk space allowed for a domain.

## Displaying contents of a shared object {#displaying-contents-of-a-shared-object}

Values are stored in shared objects within the data property. You can loop over each value within a shared object instance by using a for..in loop, as the following example shows:

var so:SharedObject = SharedObject.getLocal("test"); so.data.hello = "world";

so.data.foo = "bar";

so.data.timezone = new Date().timezoneOffset; for (var i:String in so.data)

{

trace(i + ":\t" + so.data[i]);

}

## Destroying shared objects {#destroying-shared-objects}

To destroy a SharedObject on the client, use the SharedObject.clear() method. This does not destroy directories in the default path for the application’s shared objects.

The following example deletes the SharedObject file from the client:

public function destroySharedObject():void { mySO.clear();

}

## SharedObject example {#sharedobject-example}

The following example shows that you can store simple objects, such as a Date object, in a [SharedObject](https://api.openfl.org/openfl/net/SharedObject.html) object without having to manually serialize and deserialize those objects.

The following example begins by welcoming you as a first-time visitor. When you click Log Out, the application stores the current date in a shared object. The next time you launch this application or refresh the page, the application welcomes you back with a reminder of the time you logged out.

To see the application in action, launch the application, click Log Out, and then refresh the page. The application displays the date and time that you clicked the Log Out button on your previous visit. At any time, you can delete the stored information by clicking the Delete LSO button.

&lt;?xml version="1.0"?&gt;

&lt;!-- lsos/WelcomeMessage.mxml --&gt;

&lt;mx:Application [xmlns:mx="http://www.adobe.com/2006/mxml"](http://www.adobe.com/2006/mxml) initialize="initApp()"&gt;

&lt;mx:Script&gt;&lt;![CDATA[

public var mySO:SharedObject; [Bindable]

public var welcomeMessage:String;

public function initApp():void {

mySO = SharedObject.getLocal("mydata"); if (mySO.data.visitDate==null) {

welcomeMessage = "Hello first-timer!"

} else {

welcomeMessage = "Welcome back. You last visited on " + getVisitDate();

}

}

private function getVisitDate():Date { return mySO.data.visitDate;

}

private function storeDate():void { mySO.data.visitDate = new Date(); mySO.flush();

}

private function deleteLSO():void {

// Deletes the SharedObject from the client machine.

// Next time they log in, they will be a 'first-timer'. mySO.clear();

}

]]&gt;&lt;/mx:Script&gt;

&lt;mx:Label id="label1" text="{welcomeMessage}"/&gt;

&lt;mx:Button label="Log Out" click="storeDate()"/&gt;

&lt;mx:Button label="Delete LSO" click="deleteLSO()"/&gt;

&lt;/mx:Application&gt;