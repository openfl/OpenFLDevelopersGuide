## Localizing dates, times, and currencies {#localizing-dates-times-and-currencies}

Flash Player 9 and later, Adobe AIR 1.0 and later

The way applications present dates, times, and currencies varies greatly for each locale. For example, the U.S. standard for representing dates is month/day/year, whereas the European standard for representing dates is day/month/year.

You can write code to format dates, times, and currencies. For example, the following code converts a Date object into month/day/year format or day/month/year format. if the locale variable (representing the locale) is set to &quot;en_US&quot;, the function returns month/day/year format. The example converts a Date object into day/month/year format for all other locales:

function convertDate(date)

{

if (locale == &quot;en_US&quot;)

{

return (date.getMonth() + 1) + &quot;/&quot; + date.getDate() + &quot;/&quot; + date.getFullYear();

}

else

{

return date.getDate() + &quot;/&quot; + (date.getMonth() + 1) + &quot;/&quot; + date.getFullYear();

}

}

ADOBE FLEX

The Flex framework includes controls for formatting dates, times, and currencies. These controls include the DateFormatter and CurrencyFormatter controls.

• [mx:DateFormatter](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/mx/formatters/DateFormatter.html)

• [mx:CurrencyFormatter](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/mx/formatters/CurrencyFormatter.html)