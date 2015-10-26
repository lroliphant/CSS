## Resources

[https://developer.mozilla.org/en-US/docs/Web/CSS]

[Https://docs.webplatform.org/wiki/css]

[http://caniuse.com/]



###### Using @import

```
@import 'reset-styles.css';
```
* The @import statement lets us import CSS from other style sheets. It shares some of the same advantages as linking a stylesheet, like browser caching and maintenance efficiency.
* The @import statement must precede all other CSS rules in a style sheet in order for it to work properly.


###### Line height
* We'll commonly use the line-height property in the body element to set the overall line-height of the page. For example:
```
body {
  line-height: 1.5;
}
```
* The browser multiplies the font size of each element by 1.5 to determine their line height.


###### font
* A shorthand property that lets us write all the font properties in one value.
```
body {
  font: normal 1em/1.5 "Helvetica Neue", Helvetica, Arial, sans-serif;
}
```


## Percentage and Pixel Units 

* When we use pixel units, the size we define will always remain the same and will not scale, regardless of the browser window and size of the screen.
* We can define number values more fluidly with percentage units. Percentages, by nature, are always relative to something else. 
  When we use a percentage unit in CSS, the percentage value is measured relative to a parent element's length.


#### Em and rem units

[http://www.sitepoint.com/understanding-and-using-rem-units-in-css/]

* Relative length units are relative to other length values. The most commonly used relative unit is the em unit. The em is known as a font-relative unit because it's calculated based on a parent element's font size.

###### Using em units

* If we set our body's font-size value to 1em:
```
body {
  font-size: 1em;
}
```
--> This is equivalent to setting the font-size value to:
```
body {
  font-size: 16px;
}
```

###### Converting pixels to ems

To convert a px value to its equivalent em value, we can follow a simple formula: divide an element’s px value by the parent element's font-size value.

For example:
```
.intro {
  font-size: 24px;
}
```
* To convert the font-size value of intro to an em value, we'll need to divide 24 by the parent element's font-size. In this case, the parent element is the body element, which has the font-size set to 1em, or 16px.

24/16 = 1.5
This gives us an em-based font-size value of:
```
.intro {
  font-size: 1.5em;  /* 24px/16px */
}
```

###### rem units
* The rem unit is similar to the em unit. The difference is that rem is relative only to the root element of the page. This gets us around the compounding font size issue we experience with em units.

* Using rem units:
```
h1 {
  font-size: 5.625rem;
}
```
--> The h1 font size is now relative to the default 16px font size of the root <html> element.

[http://pxtoem.com/]



#### Common Color Values

###### Hexadecimal values
```
.main-header {
  background-color: #ff0033;
}
```

###### RGB Values
```
a:link {
  color: rgb(255, 169, 73);
}
```

###### RGBa Values
```
a:hover {
  color: rgba(255, 169, 73, .4);
}
```


## The CSS Box Model 

* The box model is what describes the amount of space each element takes up on the page. Let's dig a little deeper into this concept by going over the main components that make up the CSS Box Model.
* inner element is content
* padding - creates space inside box
* border - outline or outmost part of the box
* margin area exists outside box, its the space that separates elements from one another
- so padding creates space inside the box whereas margin creates space outside the box

* By default the box model is additive; thus to determine the actual size of a box we need to take into account padding, borders, and margins for all four sides of the box. Our width not only includes the width property value, but also the size of the left and right padding, left and right borders, and left and right margins.


#### Display

###### Block elements
* default width of 100% - consuming the entire horizontal width available
for a separate block that takes up the full width available to it depending on its parent element 
* creates a new line before and after the element accept the width and height properties and their corresponding values
* eg. div, p, li (list items), h1-h5 (header elements)

###### Inline elements
* only take up the width they need
* don’t create new lines
* inline-level elements will not accept the width and height properties or top and bottom padding and margins
* eg. span tags, a (anchor elements) and images
* minor white space issue also happens with inline elements


#### Inline-block elements
* using this value allows an element to behave as a block-level element, accepting all box model properties. However, the element will be displayed in line with other elements, it will not begin on a new line by default
* accept the width and height properties and their corresponding values


#### Padding, Borders, Margin

###### Padding
```
.wildlife {
  padding: 100px 120px 50px 20px;  
}
```

###### Border


######  Margin
```
.wildlife {
  margin: 105px 0 60px 0;
}
```

## Basic Layout
 
* By default, an element's width and height are as wide or as tall as the content it holds. But we're able to set our own dimensions with the width and height properties.


#### box-sixing and max-width

* With the box-sizing property, we can alter the way the browser calculates an element's total width and height. We're also able to set the maximum width of an element with the max-width property.


###### box-sizing

* Alters the default CSS box model used to calculate widths and heights of elements.
* The value border-box forces the padding and borders into the width and height instead of expanding it:
```
.box {
   width: 300px;
   padding: 20px;
   border: 10px solid #478D93;
   box-sizing: border-box;
}
```
* The padding and borders are laid out and drawn inside the width and height. So it dynamically subtracts the borders and padding of each side from the width and height properties we determine.
```
* {
  box-sizing: border-box;
}
```

-> generally it is bad practice to use universal selectors but in general it might be worth using a universal selector in this case */

###### max-width
* Sets the maximum width of an element. It prevents the used value of the width property from becoming larger than the value set for max-width. */
```
img {
  max-width: 100%;
}
```

[http://blog.teamtreehouse.com/take-control-of-the-box-model-with-box-sizing]


#### Backgrounds: color and images

* Every HTML element has a background layer that is transparent by default.

###### background-image
* Sets one or several background images for an element.

```
.main-header {
  background-image: url("https://mdn.mozillademos.org/files/6457/mdn_logo_only_color.png");
}
```
###### background-size
* Sets the size of a background image.

###### background-repeat
* Controls whether or not the image is tiled and how it gets tiled both vertically and horizontally. The value repeat-y repeats the image vertically, while repeat-x repeats the background image horizontally.

###### background shortcut
```
.topbanner { 
  background: url("topbanner.png") #00D repeat-y fixed; 
}
```

#### Backgrounds: size and position

###### background-position
* Controls the background position of a background image. By default, a background image’s initial position is the top-left corner of the containing element.

###### background
* Shorthand for setting the individual background values in one place.

###### cover
* proportionally scales the image - ensures background image is displayed at a scaled size
* The cover value adjusts the background area so that it's completely covered by the background image, while maintaining its width and height proportions:
```
.main-header {
  background-image: url('../img/mountains.jpg');     
  background-size: cover;
}
```

* shortcut for background
```
.main-header {
  background: #ffa949 url('../img/mountains.jpg') no-repeat center;
  background-size: cover; /* more readable to keep it seperate but it can be added above */
}
```

#### Floats

[http://www.smashingmagazine.com/2007/05/css-float-theory-things-you-should-know/]

* Floats are one of the most commonly used methods for laying out a page with CSS. When an element is floated, the element is taken out of the normal flow of the page and placed along the left or right side of its container.

The following will float an element over to the right side of its container:
```
.tips {
  float: right;
}
``` 

The following will place the element along the left side of its container:
```
.tips {
  float: left;
}
```

###### Clearing Floats
If a block element contains floated children, its height will collapse. We'll learn two methods for accommodating floated elements.

Fixes for collapsed content caused by use of floats:
* overflow - overflow: auto; (issues can cause scrollbars to appear in some browsers but ok for simple layouts as a fix, not perhaps the best fix though)
* clear - clear: both;
* the clearfix hack
* content: ''

```
.group:after {
  content: "";
  display: table;
  clear: both;
}
```


## Media Queries

* With media queries, we're able to enhance the browsing experience of websites on multiple devices and viewport sizes. This allows us to tailor our content to a wide range of devices without having to change anything in the HTML.
```
@media (max-width: 960px) {
  p { color: #fff; }
}

@media (max-width: 480px) {
  p { color: #000; }
}

@media (min-width: 481px) and (max-width: 700px) {
  p { color: seagreen; }
}
```
