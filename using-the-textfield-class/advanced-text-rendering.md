# Advanced text rendering

OpenFL provides a variety of classes in the [openfl.text package](https://api.openfl.org/openfl/text/index.html) to control the
properties of displayed text, including embedded fonts, anti-aliasing settings,
alpha channel control, and other specific settings. The OpenFL API Reference
provides detailed descriptions of these classes and properties, including the
[Font class](https://api.openfl.org/openfl/text/Font.html).

## Using embedded fonts

When you specify a specific font for a TextField in your application, OpenFL
looks for a device font (a font that resides on the user's computer) with the
same name. If it doesn't find that font on the system, or if the user has a
slightly different version of a font with that name, the text display could look
very different from what you intend. By default, the text appears in a Times
Roman font.

To make sure the user sees exactly the right font, you can embed that font in
your application. Embedded fonts have a number of benefits:

- Embedded font characters are anti-aliased, making their edges appear smoother,
  especially for larger text.

- You can rotate text that uses embedded fonts.

- Embedded font text can be made transparent or semitransparent.

- You can use the `kerning` CSS style with embedded fonts.

The biggest limitation to using embedded fonts is that they increase the file
size or download size of your application.

Once you have embedded a font you can make sure a TextField uses the correct
embedded font:

- Set the `embedFonts` property of the TextField to `true`.

- Create a TextFormat object, set its `fontFamily` property to the name of the
  embedded font, and apply the TextFormat object to the TextField. When
  specifying an embedded font, the `fontFamily` property should only contain a
  single name; it cannot use a comma-delimited list of multiple font names.

- If using CSS styles to set fonts for TextFields or components, set the
  `font-family` CSS property to the name of the embedded font. The `font-family`
  property must contain a single name and not a list of names if you want to
  specify an embedded font.

#### Embedding a font

In a
[Lime _project.xml_ file](https://lime.openfl.org/docs/project-files/xml-format/),
include font files using the
[`<assets>` element](https://lime.openfl.org/docs/project-files/xml-format/#assets).

```xml
<assets type="font" path="path/to/font.ttf"/>
```
