# computer vision for non-stem majors: all you ever wanted to know 

by michael valentino ochoa

## REASONS FOR THIS COOKBOOK

* This document was written for non-stem learners interested in computer vision.  The goal is provide readers with a working understanding of computer vision. I presume you have high school / early college math literacy. I also assume a knowledge of C and programming, as examples are coded in C.  
* This cookbook is filled with code for low level computer vision applications. I organize the document around dialing up the complexity bit by bit to demonstarate code in action. All problems are broken down into smaller digestable components, and then put together in a way yielding fresh tools and insights. For the time being we avoid three dimensional vision, and largely avoid RBG color maps.  My hope is to show curious non-stem students the basics of identifying and classifying objects into themed groups.

## BASICS OF COMPUTER VISION (BI-LEVEL IMAGES)

* This document characterizes computer vision problems as numerical ones. We start simple using **bi-level images** that compress the range of greys into two levels, with one bit per pixel.  This approach helps us build a sturdy foundation before moving onto more complex analysis. We begin with a **Raster Image**, defined as a 2D array of pixels representing an images level of grey (0 assigned to black, 255 to write) and its vertical and horizontal positions (256 row pixels x 256 column pixels).  This number of grey levels was chosen as the human vision system does not need more than 256 grey levels. Each raster image is associated to a **header record** of desciption / start byte / end byte, so that future code can operate on any image size and flexibly grow as needed. 

* Q: Why is computing the perimeter of an object tricky?
* A1: Pixels represent area, not a linear distance. (e.g. specifying the distance between Detroit and Tangier in acres is irrelevant to most people)
* A2: Pixels only represent grey level, not any other info. 
* A3: The length between pixels is represented by their vertical, horiztonal and diagonal relationships (i.e. if neighbors vertical and horizontal then 1; if diagonal then 1.414; if one diagonal and one non-diagonal then 1.207). This means there is an implict margin of error in sampling that means the "real" perimeter is never exact, a higher resolution grid is the best way to improved accuracy.

* Q: How do we calculate the basic shape of an object using an object-oriented enclosing rectangle?
* A: First, we calculate the object's **center of mass** to generalize the shape of region and provide us a standard position. Next we orient the object by finding its **principal axis (PA)** - the line passing thru the objects center of mass having the minimum total distance from all pixels belonging to the object. We follow that up by collecting the **minor axis (MA)** - the line passing thru the center of mass perpendicular to the (PA).  With those players up we then compute the distance between the line given and all pixels in that region to construct the object oriented encolsing rectangle. This approach rests on the idea that complex functions can usually be described in terms of simplier shapes forming a hierarchy.

```
/* Calculate distance between line given and all pixels in region */
/* Line given made up of two points: (i1, j1) and (i2, j2). */

float all_dist (struct image *x, float i1, float j1, float i2, float j2, int val)

{
    int i, j, k; 
    float a, b, c, e, f, d;

/* Equation of the line is a*x + b*y + c = 0 */
    a = (float)j2-(float)j1;
    b = (float)i1-(float)i2;
    c = (float) ( -(i1-i2)*j1 + (j1-j2)*i1 );
    e = a*a + b*b;
    d = 0.0;

/* Sum residuals, substitute (i, j) for each pixel in place of (x,y)
    for (i=0; i<x->info->nr; i++)
      for (j=0; j<x->info->nc; j++) {
        if (x->data[i][j] != val)continue;
        f = (a*i + b*j + c);
        f = f*f/e;
        d = d + f;
      }
    return d;
}

/* Compute distances between line given and all pixels in region; return pixels with min/max distances */

/* Equation of the line */

/* Locate pixels with maximum and minimum residuals */



