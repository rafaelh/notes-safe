/* CSS is normally placed in a file called style.css */

/* To select all elements of a particular type, use the following: */

p {
  /* Declarations of all Properties and Values for all <p> tags */
  font-size: 20px;
  color: Blue;
}

/* You can select multiple elements this way: */

h1, h2, p {
  color: Green;
}

/* Or you can select every element this way: */

* {
  font-family: Arial;
  color: #FFFFFF;
  color: #FFF; /* This is an abreviation of the above colour */
}

/* ================================================= */
/* Colours: */

h1 {
  color: Red;
  background-color: Blue;
}

/* Setting values via RGB: */

h1 {
  color: rgb(123, 20, 233);
  background-color: rgb(99, 21, 127);
}

h1 {
  color: hsl(182, 20%, 50%);
}

/*
HSL stands for Hue, Saturation, and Lightness. Specifically, this is what each means:

Hue - the technical term that describes what we understand as "color." In HSL, hue is represented 
on a color wheel. It can take on values between 0 and 360.

Saturation - the amount of gray in a given color. In HSL, saturation is specified using a percentage 
between 0% and 100%. The percentage 0% represents a shade of gray, whereas 100% represents full saturation.

Lightness - the amount of white in a given color. Similar to saturation, lightness is specified using 
a percentage between 0% and 100%. The percentage 0% represents black, whereas 100% represents white. 
50% is normal.
*/

h1 {
  color: rgb(123, 88, 9); /* Redundant color declaration for older browsers */
  color: rgba(123, 88, 9, 0.5); 
  /* The 'a' value is set between 0 and 1, and determines opacity. Later declarations take priority */
  color: hsla(182, 20%, 50%, 0.5); /* This can be used for HSL too */}


<link href="https://fonts.googleapis.com/css?family=Bellefair" rel="stylesheet">
font-family: 'Bellefair', serif;

<link href="https://fonts.googleapis.com/css?family=Quicksand" rel="stylesheet">
font-family: 'Quicksand', Arial, sans-serif; /* The second and third fonts are fallback fonts */

h1 {
  color: #FFF;
  font-size: 32px;
  padding-top: 100px;
  text-align: left;
  font-style: italic;
  text-transform: uppercase;
  font-weight: bold; /* Turns bold on or off */
  line-height: 1.7em; /* Sets the leading space before a line */
  word-spacing: 0.3em; /* Space between words */
  letter-spacing: 0.3em; /* Changes kerning */
  width: 60%; /* How much of the screen is used */
  border-bottom: 1px solid #EEE; /* Creates a solid line */
}

#botswana {
  background-color: #56ABFF;
} /* Styling for a specific element, using the id="" selector */


.science, .education {
  font-family: Georgia, Times, serif;
  color: #A3B4C5;
  text-transform: uppercase;
} /* Styling for a series of elements, using the class="science" selector, or the class="education" selector */

/* You can create a class, then further define more styles for particular elements within that class: */
.breaking {
  font-family: Georgia, Times, serif;
}

p.breaking {
  line-height: 1.3em;
}

/* Element boxes have two dimensions, height and width, which can be set in px, em, or % */
.content-box {
  height: 500px;
  width: 80%;
  background-image: url("https://s3.amazonaws.com/codecademy-content/courses/web-101/unit-6/htmlcss1-img_tahoe.jpeg");
  max-width: 80%; /* Min & Max settings can be used for responsive pages */
  min-width: 40%;
  max-height: 500px;
  min-height: 300px;
  overflow: scroll; /* Controls what happens to content that doesn't fit. Scroll or Hidden. */
  border: 1px solid rgba(0, 0, 0, 0.3);
  /* This sets a border, and it's color. It can have the following style settings:
    solid - border is a solid line.
    dashed - border is a series of lines or dashes.
    dotted - border is a series of square dots.
    double - border is two solid black lines.
    groove - border is a groove (or carving).
    inset - border appears to cut into the screen.
    outset - border appears to pop out of the screen.
    ridge - border appears as a picture frame.
    hidden or none - no border.

    The width can also be set to thin/medium/thick, or specified in px
  */
  border-radius: 10px; /* Sets corner rounding. 100% is a circle */
}

p {
  border: 3px solid #A2D3F4;
  padding: 10px; /* Space between content and edge of box */
  padding: 5px 10px 5px 10px; /* Differing padding by side */
  margin: 20px; /* Adds padding outside the box. Set to 'auto' to center to the width */
}

* {
  margin: 0;
  padding: 0;
} /* Resets all CSS rules to avoid browser defaults. Often used as first CSS rule */


/* All HTML elements are either inline (link) or block (div) elements. This can be
changed in CSS:

inline - causes block-level elements (like a div) to behave like an inline element (like a link) so 
         it tiles accross the screen.
block -  causes inline elements (like a link) to behave like a block element (like a div).
inline-block - causes block-level elements to behave like an inline element, but retain the features 
         of a block-level element, like keeping it's padding.
none -   removes an element from view. The rest of the web page will act as if the element does not exist.
*/

li {
  display: inline;
  visibility: hidden; /* Allows hiding an element. Opposite is 'visible' */
  box-sizing: border-box; /* In this box model, height and width remain fixed. Border thickness and 
  padding will be included inside of the box, so the dimensions do nor change.
  This is a good candidate to be done with * {} */
}

/* The default position of an element can be changed by setting it's position property:

static - the default
relative
absolute - other elements will ignore this one, and probably go under it
fixed - this element will ignore all others, and remain fixed on the page when the user scrolls - like a header

*/

.box {
  background-color: DeepSkyBlue;
  position: relative;
  top: 20px; /* These modify the relative position. Bottom & Right can also be set */
  left: 50px;
  z-index: 1; /* Controls how far forward or backward overlapping elements should be */
  float: right; /* Moves the element as far right as possible. 'left' can also be set */
  clear: left; /* Sets how elements behave when they bump into each other. 'left', 'right', 'both', 'none' */
}

body {
  background-image: url("https://www.example.com/leaf.jpg");
  background-repeat: repeat; /* Background will repeat horizontally
  and vertically. Other options are repeat-x and repeat-y, which tile
  only in one direction, and no-repeat */
  background-position: left top; /* or centre top, right top, left
  center, center center, right center, left bottom, center bottom,
  right bottom */
  /* Or set all three at once: */
  background: url("#") no-repeat right center;
  background-size: cover; /* 'Cover' expands the image to fill the full width or height of the
  container. 'Contain' letterboxes the image so that it doesn't cover the full width or height */
  background-attachment: fixed; /* 'Fixed' pins the image on the page, 'Scroll' allows it to move */
  background-image: -webkit-linear-gradient(#666CCC, #BC1324); /* Creates a gradient. Not supported
  in all browsers */
}

