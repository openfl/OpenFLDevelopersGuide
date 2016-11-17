## Printing example: Scaling, cropping, and responding {#printing-example-scaling-cropping-and-responding}

Flash Player 9 and later, Adobe AIR 1.0 and later

In some cases, you want to adjust the size (or other properties) of a display object when printing it to accommodate differences between the way it appears on screen and the way it appears printed on paper. When you adjust the properties of a display object before printing (for example, by using the scaleX and scaleY properties), be aware that if the object scales larger than the defined rectangle for the print area, the object is cropped. You will also probably want to reset the properties after the pages have been printed.

The following code scales the dimensions of the txt display object (but not the green box background), and the text field ends up cropped by the dimensions of the specified rectangle. After printing, the text field is returned to its original size for display on screen. If the user cancels the print job from the operating system’s Print dialog box, the content in the Flash runtime changes to alert the user that the job has been canceled.

package

{

import flash.printing.PrintJob; import flash.display.Sprite; import flash.text.TextField; import flash.display.Stage; import flash.geom.Rectangle;

public class PrintScaleExample extends Sprite

{

private var bg:Sprite; private var txt:TextField;

public function PrintScaleExample():void

{

init();

draw(); printPage();

}

private function printPage():void

{

var pj:PrintJob = new PrintJob(); txt.scaleX = 3;

txt.scaleY = 2; if (pj.start())

{

trace(&quot;&gt;&gt; pj.orientation: &quot; + pj.orientation); trace(&quot;&gt;&gt; pj.pageWidth: &quot; + pj.pageWidth); trace(&quot;&gt;&gt; pj.pageHeight: &quot; + pj.pageHeight); trace(&quot;&gt;&gt; pj.paperWidth: &quot; + pj.paperWidth); trace(&quot;&gt;&gt; pj.paperHeight: &quot; + pj.paperHeight);

try

{

}

pj.addPage(this, new Rectangle(0, 0, 100, 100));

catch (error:Error)

{

// Do nothing.

}

pj.send();

}

else

{

txt.text = &quot;Print job canceled&quot;;

}

// Reset the txt scale properties. txt.scaleX = 1;

txt.scaleY = 1;

}

private function init():void

{

bg = new Sprite(); bg.graphics.beginFill(0x00FF00); bg.graphics.drawRect(0, 0, 100, 200); bg.graphics.endFill();

txt = new TextField(); txt.border = true; txt.text = &quot;Hello World&quot;;

}

private function draw():void

{

addChild(bg); addChild(txt); txt.x = 50;

txt.y = 50;

}

}

}