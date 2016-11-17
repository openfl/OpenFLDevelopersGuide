# Chapter 57: Localizing applications {#chapter-57-localizing-applications}

Flash Player 9 and later, Adobe AIR 1.0 and later

Localization is the process of including assets to support multiple locales. A locale is the combination of a language and a country code. For example, en_US refers to the English language as spoken in the United States, and fr_FR refers to the French language as spoken in France. To localize an application for these locales, you would provide two sets of assets: one for the en_US locale and one for the fr_FR locale.

Locales can share languages. For example, en_US and en_GB (Great Britain) are different locales. In this case, both locales use the English language, but the country code indicates that they are different locales, and might therefore use different assets. For example, an application in the en_US locale might spell the word &quot;color&quot;, whereas the word would be &quot;colour&quot; in the en_GB locale. Also, units of currency would be represented in dollars or pounds, depending on the locale, and the format of dates and times might also be different.

You can also provide a set of assets for a language without specifying a country code. For example, you can provide en assets for the English language and provide additional assets for the en_US locale, specific to U.S. English.

Localization goes beyond just translating strings used in your application. It can also include any type of asset such as audio files, images, and videos.

**Choosing a locale**

Flash Player 9 and later, Adobe AIR 1.0 and later

To determine which locale your content or application uses, you can use one of the following methods:

• flash.globalization package — Use the locale-aware classes in the flash.globalization package to retrieve the default locale for the user based on the operating system and user preferences. This is the preferred approach for applications that will run on the Flash Player 10.1 or later or AIR 2.0 or later runtimes. See

“Determining the locale”

on page 944

for more information.

• User prompt — You can start the application in some default locale, and then ask the user to choose their preferred locale.

• (AIR only) Capabilities.languages — The Capabilities.languages property lists an array of languages available on the user’s preferred languages, as set through the operating system. The strings contain language tags (and script and region information, where applicable) defined by RFC4646 ([http://www.ietf.org/rfc/rfc4646.txt](http://www.ietf.org/rfc/rfc4646.txt)). The strings use hyphens as a delimiter (for example, &quot;en-US&quot; or &quot;ja-JP&quot;). The first entry in the returned array has the same primary language ID as the language property. For example, if languages[0] is set to &quot;en-US&quot;, then the language property is set to &quot;en&quot;. However, if the language property is set to &quot;xu&quot; (specifying an unknown language), the first element in the languages array is different.

• Capabilities.language — The Capabilities.language property provides the user interface language code of the operating system. However, this property is limited to 20 known languages. And on English systems, this property returns only the language code, not the country code. For these reasons, it is better to use the first element in the Capabilities.languages array.