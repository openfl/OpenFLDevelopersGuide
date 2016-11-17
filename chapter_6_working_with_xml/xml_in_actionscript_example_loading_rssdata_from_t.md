## XML in ActionScript example: Loading RSSdata from the Internet {#xml-in-actionscript-example-loading-rssdata-from-the-internet}

Flash Player 9 and later, Adobe AIR 1.0 and later

The RSSViewer sample application shows a number of features of working with XML in ActionScript, including the following:

*   Using XML methods to traverse XML data in the form of an RSS feed.
*   Using XML methods to assemble XML data in the form of HTML to use in a text field.

The RSS format is widely used to syndicate news via XML. A simple RSS data file may look like the following:

&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;

&lt;rss version=&quot;2.0&quot; [xmlns:dc=&quot;http://purl.org/dc/elements/1.1/&quot;&gt;](http://purl.org/dc/elements/1.1/)

&lt;channel&gt;

&lt;title&gt;Alaska - Weather&lt;/title&gt;

[&lt;link&gt;http://www.nws.noaa.gov/alerts/ak.html&lt;/link&gt;](http://www.nws.noaa.gov/alerts/ak.html)

&lt;description&gt;Alaska - Watches, Warnings and Advisories&lt;/description&gt;

&lt;item&gt;

&lt;title&gt;

Short Term Forecast - Taiya Inlet, Klondike Highway (Alaska)

&lt;/title&gt;

&lt;link&gt;

[http://www.nws.noaa.gov/alerts/ak.html#A18.AJKNK.1900](http://www.nws.noaa.gov/alerts/ak.html#A18.AJKNK.1900)

&lt;/link&gt;

&lt;description&gt;

Short Term Forecast Issued At: 2005-04-11T19:00:00

Expired At: 2005-04-12T01:00:00 Issuing Weather Forecast Office Homepage: [http://pajk.arh.noaa.gov](http://pajk.arh.noaa.gov/)

&lt;/description&gt;

&lt;/item&gt;

&lt;item&gt;

&lt;title&gt;

Short Term Forecast - Haines Borough (Alaska)

&lt;/title&gt;

&lt;link&gt; [http://www.nws.noaa.gov/alerts/ak.html#AKZ019.AJKNOWAJK.190000](http://www.nws.noaa.gov/alerts/ak.html#AKZ019.AJKNOWAJK.190000)

&lt;/link&gt;

&lt;description&gt;

Short Term Forecast Issued At: 2005-04-11T19:00:00

Expired At: 2005-04-12T01:00:00 Issuing Weather Forecast Office Homepage: [http://pajk.arh.noaa.gov](http://pajk.arh.noaa.gov/)

&lt;/description&gt;

&lt;/item&gt;

&lt;/channel&gt;

&lt;/rss&gt;

The SimpleRSS application reads RSS data from the Internet, parses the data for headlines (titles), links, and descriptions, and returns that data. The SimpleRSSUI class provides the UI and calls the SimpleRSS class, which does all of the XML processing.

To get the application files for this sample, see [www.adobe.com/go/learn_programmingAS3samples_flash](http://www.adobe.com/go/learn_programmingAS3samples_flash). The RSSViewer application files can be found in the folder Samples/RSSViewer. The application consists of the following files:

| **File** | **Description** |
| --- | --- |
| RSSViewer.mxml or | The main application file in Flash (FLA) or Flex (MXML). |
| com/example/programmingas3/rssViewer/RSSParser.as | A class that contains methods that use E4X to traverse RSS (XML) data and generate a corresponding HTML representation. |
| RSSData/ak.rss | A sample RSS file. The application is set up to read RSS data from the web, at a Flex RSS feed hosted by Adobe. However, you can easily change the application to read RSS data from this document, which uses a slightly different schema than that of the Flex RSS feed. |

**Reading and parsing XML data**

Flash Player 9 and later, Adobe AIR 1.0 and later

The RSSParser class includes an xmlLoaded() method that converts the input RSS data, stored in the rssXML variable, into an string containing HTML-formatted output, rssOutput.

Near the beginning of the method, code sets the default XML namespace if the source RSS data includes a default namespace:

if (rssXML.namespace(&quot;&quot;) != undefined)

{

default xml namespace = rssXML.namespace(&quot;&quot;);

}

The next lines then loop through the contents of the source XML data, examining each descendant property named item: for each (var item:XML in rssXML..item)

{

var itemTitle:String = item.title.toString();

var itemDescription:String = item.description.toString(); var itemLink:String = item.link.toString();

outXML += buildItemHTML(itemTitle,

itemDescription, itemLink);

}

The first three lines simply set string variables to represent the title, description and link properties of the item property of the XML data. The next line then calls the buildItemHTML() method to get HTML data in the form of an XMLList object, using the three new string variables as parameters.

### Assembling XMLList data {#assembling-xmllist-data}

Flash Player 9 and later, Adobe AIR 1.0 and later

The HTML data (an XMLList object) is of the following form:

&lt;b&gt;itemTitle&lt;/b&gt;

&lt;p&gt;

itemDescription

&lt;br /&gt;

&lt;a href=&quot;link&quot;&gt;

&lt;font color=&quot;#008000&quot;&gt;More...&lt;/font&gt;

&lt;/a&gt;

&lt;/p&gt;

The first lines of the method clear the default xml namespace:

default xml namespace = new Namespace();

The default xml namespace directive has function block-level scope. This means that the scope of this declaration is the buildItemHTML() method.

The lines that follow assemble the XMLList, based on the string arguments passed to the function:

var body:XMLList = new XMLList();

body += new XML(&quot;&lt;b&gt;&quot; + itemTitle + &quot;&lt;/b&gt;&quot;);

var p:XML = new XML(&quot;&lt;p&gt;&quot; + itemDescription + &quot;&lt;/p&gt;&quot;);

var link:XML = &lt;a&gt;&lt;/a&gt;;

link.@href = itemLink; // &lt;link href=&quot;itemLinkString&quot;&gt;&lt;/link&gt; link.font.@color = &quot;#008000&quot;;

// &lt;font color=&quot;#008000&quot;&gt;&lt;/font&gt;&lt;/a&gt;

// 0x008000 = green link.font = &quot;More...&quot;;

p.appendChild(&lt;br/&gt;); p.appendChild(link); body += p;

This XMLList object represents string data suitable for an ActionScript HTML text field.

The xmlLoaded() method uses the return value of the buildItemHTML() method and converts it to a string:

XML.prettyPrinting = false; rssOutput = outXML.toXMLString();

### Extracting the title of the RSS feed and sending a custom event {#extracting-the-title-of-the-rss-feed-and-sending-a-custom-event}

Flash Player 9 and later, Adobe AIR 1.0 and later

The xmlLoaded() method sets a rssTitle string variable, based on information in the source RSS XML data:

rssTitle = rssXML.channel.title.toString();

Finally, the xmlLoaded() method generates an event, which notifies the application that the data is parsed and available:

dataWritten = new Event(&quot;dataWritten&quot;, true);