<div align="center">

## Java Pie Chart


</div>

### Description

Just a cool pie chart... VOTE FOR ME ANYWAYS, please! :)
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Mike McNaughton](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/mike-mcnaughton.md)
**Level**          |Intermediate
**User Rating**    |4.6 (23 globes from 5 users)
**Compatibility**  |Java \(JDK 1\.2\)
**Category**       |[Applet](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/applet__2-81.md)
**World**          |[Java](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/java.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/mike-mcnaughton-java-pie-chart__2-2085/archive/master.zip)





### Source Code

```
/* Pie chart Applet */
/* This program is in the public domain.
*/
import java.awt.*;
import java.applet.*;
import java.util.*;
import java.lang.Math;
// ================================================================
// A struct class to hold data for one wedge
class PieItem {
 public double frac; // each one has a number
 public String label; // and a label
 PieItem (String s) { // constructor
  StringTokenizer t = new StringTokenizer(s, ",");
  frac = Double.valueOf(t.nextToken()).doubleValue();
  label = t.nextToken();
 } // constructor
} // PieItem
//The view of the pie.
class PieView extends Canvas {
 PieItem[] wedges; // The data for the pie
 double total = 0.0; // Total of all wedges
 static final int ncolors = 5;
 Color wedgeColor[] = new Color[5];
 int pieViewSize; // size of square to incise pie into
 static final int pieBorderWidth = 10; // pixels from circle edge to side
 int pieDiameter; // derived from the view size
 int pieRadius; // ..
 int pieCenterPos; // ..
 public PieView(int asize, PieItem[] avec) { // constructor
  this.pieViewSize = asize; // copy args
  this.wedges = avec;
  pieDiameter = pieViewSize-2*pieBorderWidth;
  pieRadius = pieDiameter/2;
  pieCenterPos = pieBorderWidth+pieRadius;
  this.setFont(new Font("Helvetica",Font.BOLD,12));
  this.setBackground(Color.white);
  for (int i = 0; i<wedges.length; i++) {
   total += wedges[i].frac;
  }
  wedgeColor[0] = Color.green; // colors that black looks good on
  wedgeColor[1] = Color.pink;
  wedgeColor[2] = Color.cyan;
  wedgeColor[3] = Color.red;
  wedgeColor[4] = Color.yellow;
 } // constructor
 public void paint(Graphics g) {
  int startDeg = 0;
  int arcDeg;
  int x, y;
  double angleRad;
  g.setColor(Color.lightGray); // shadow
  g.fillOval(pieBorderWidth+3,pieBorderWidth+3,pieDiameter,pieDiameter);
  g.setColor(Color.gray); // "other" is gray
  g.fillOval(pieBorderWidth,pieBorderWidth,pieDiameter,pieDiameter);
  int wci = 0;
  int i;
  for (i = 0; i<this.wedges.length; i++) { // draw wedges
   arcDeg = (int)((this.wedges[i].frac / total) * 360);
   g.setColor(wedgeColor[wci++]);
   g.fillArc(pieBorderWidth,pieBorderWidth,pieDiameter,pieDiameter,
		startDeg, arcDeg);
   if (wci >= ncolors) {
	wci = 0; // rotate colors
   }
   startDeg += arcDeg;
  } // draw wedges
  startDeg = 0; // Do labels so they go on top of the wedges.
  for (i = 0; i<this.wedges.length; i++) {
   arcDeg = (int)((this.wedges[i].frac / total) * 360);
   if (arcDeg > 3) { // don't label small wedges
	g.setColor(Color.black);
	angleRad = (float) (startDeg+(arcDeg/2))* java.lang.Math.PI / 180.0;
	x = pieCenterPos + (int)((pieRadius/1.3)*java.lang.Math.cos(angleRad));
	y = pieCenterPos - (int)((pieRadius/1.3)*java.lang.Math.sin(angleRad))
      + 5; // 5 is about half the height of the text
	g.drawString(this.wedges[i].label, x, y);
   } // don't label small wedges
   startDeg += arcDeg;
  } // for
 } // paint()
 public Dimension preferredSize () {
  return new Dimension (pieViewSize,pieViewSize);
 } // preferredSize
} // PieView
// ================================================================
// The Pie chart applet
public class Pie extends Applet {
 private PieView the_pie = null;
 public Pie() { // constructor
  // Nothing happens here, can't get args yet.
 } // constructor
 public void init () {
  String stemp;
  double dtemp;
  int i;
  // Read the applet arguments
  stemp = this.getParameter("title");
  String chartTitle = (stemp == null) ? "" : stemp;
  stemp = this.getParameter("subtitle");
  String chartSubTitle = (stemp == null) ? "" : stemp;
  int nargs = 0;
  while (this.getParameter("arg"+nargs) != null) {
   nargs++; // just count the arguments
  } // while
  PieItem[] v = new PieItem[nargs]; // allocate storage
  for (i=0; i<nargs; i++) {
   v[i] = new PieItem(getParameter("arg"+i)); // parse argument
  } // for
  int d = (i+1)/2; // Shell sort
  do {
   for (i=0; i < nargs-d; i++) {
	if (v[i].frac < v[i+d].frac) {
	 dtemp = v[i].frac; // swap
	 v[i].frac = v[i+d].frac;
	 v[i+d].frac = dtemp;
	 stemp = v[i].label;
	 v[i].label = v[i+d].label;
	 v[i+d].label = stemp;
	}
   } // for
   d -= 1;
  } while (d > 0);
  int h = this.size().height;
  int w = this.size().width;
  the_pie = new PieView(h-50, v); // shd be min(h-50,w)?
  this.setLayout(new BorderLayout(0,0));
  this.setBackground(Color.white);
  this.add("Center", the_pie);
  this.add("North", new Label(chartTitle));
  this.add("South", new Label(chartSubTitle));
 } // init
 // Boiler plate for HotJava
 public String getAppletInfo () {
  return "Pie 1997-06-14 THVV";
 } // getAppletInfo
 public String [][] getParameterInfo () {
  String [][] info = {
  };
  return info;
 } // getParameterInfo
 // Main program for testing only.
 public static void main(String args[]) {
  Frame f = new Frame("Pie");
  Pie p = new Pie();
  p.init();
  p.start();
  f.add("Center",p);
  f.resize(512,512);
  f.show();
 } // main
} // Pie
```

