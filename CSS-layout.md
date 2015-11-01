##  DISPLAY MODES


### Reset methods


[http://necolas.github.io/normalize.css/]

- headings by default have a top and bottom margin that normalize doesn't reset

[http://meyerweb.com/eric/tools/css/reset/]


### Margin collapsing issue

- collapsing margins also occur when the margin of a child element crosses the margin of its parent 


* Fixes:
  Add something solid to the parent - ie padding or border, to allow both margins to be used
* Add padding to the parent - adding 1px of padding allows both margins to be used
```
.padding {
  padding: 1px 0;
}
```
[https://css-tricks.com/what-you-should-know-about-collapsing-margins/]
[http://csswizardry.com/2012/06/single-direction-margin-declarations]

* Since there is no border, padding, or content to separate the top margin of our logo with the top margin of the body, we're seeing a margin collapsing issue in the header. Let's fix this by giving .main-header 5px of padding on each side. */


### List items and links

#### Inline and inline-block list items - issue with white space **/

* one solution to this is the apply a negative margin to these list items
.main-nav li {
  margin-right: -4px;
}

* To prevent inline list items from wrapping and moving onto next line use:
```
.main-header {
  white-space: nowrap;
}
```

* alternatively use inline-block on list items rather than inline


* List item links only link the text - to make the whole area clickable - will need to set the anchor elements to block or inline block and add padding to them so tha targets for the hover state are larger, as display values are not inherited by child elements and a's are inline
```
.main-nav { 
  display: block; 
  padding: 10px 20px; 
}
```

### Display mode: table
*  allows for easy vertical alignment of elements to the middle of its parent container 
```
.main-header {
  display: table;
  width: 100%;
}

.main-nav,
main-logo {
  display: table-cell;
  vertical-align: middle;
}
```
N.B. can use padding but not margins on elements displayed as table-cells

```
p > span {}
```

* CSS child selector - applying the style that follows to all <span> tags that are children of a <p> tag.
* Note that "child" means "immediate descendant", not just any descendant. p span {} is a descendant selector, applying the style that follows to all <span> tags that are children of a <p> tag or recursively children of any other tag that is a child/descendant of a <p> tag. P > SPAN only applies to SPAN tags that are children of a P tag.


### Column Layout with Inline-Block

```
* {
  box-sizing: border-box;
}

.col {
  display: inline-block;
  padding: 20px;
  margin-right: -5px;
  vertical-align: top;
}

.primary-content {
  width: 60%;
}

.secondary-content {
  width: 40%;
}
```

-> like float-based layout but without the collapsing problem


### Sticky footer

```
html,
body,
.main-wrapper,
.col {
  height: 100%;
}
```
* height of 100% is based on the height of the parent element




## FLOAT LAYOUT

* Content wraps around floated elements
* Floating an element takes it out of the normal document flow, which sometimes causes the parent element to collapse, because it no longer honours the space of the content inside it

#### Clear fix
```
.group:after {
  content: "";
  display: table;
  clear: both;
}
```


## POSITIONING

* everything is a block in CSS 
* position is used to get these blocks where you want them


### Relative positioning

* What it really means is "relative to itself or it's original position
* elements are still in normal document flow
* If you set position: relative; on an element but no other positioning attributes (top, left, bottom or right), it will no effect on it's positioning at all, it will be exactly as it would be if you left it as position: static; But if you DO give it some other positioning attribute, say, top: 10px;, it will shift it's position 10 pixels DOWN from where it would NORMALLY be - offset from original position.
* introduces the ability to use z-index on that element - even if you don't set a z-index value, this element will now appear on top of any other statically positioned element
* limits the scope of absolutely positioned child elements. Any element that is a child of the relatively positioned element can be absolutely positioned within that block
* fix for any overflow issues where the relatively positioned children are still block level elements that because they are offset overflow out of the parent - possible causing white space to one side and horizontal scrollbars: give parent overflow: hidden value
```
.main-header { 
  overflow: hidden
}
```

### Absolute positioning

* allows you to literally place any page element exactly where you want it. You use the positioning attributes top, left bottom and right to set the location.
* Remember that these values will be relative to the next/first parent element with relative (or absolute) positioning. If there is no such parent, it will default all the way back up to the <html> element itself meaning it will be placed relatively to the page itself - the browser viewport
* BUT these elements are removed from the flow of elements on the page - an element with this type of positioning is not affected by other elements and it doesn't affect other elements
* must also state a height/min-height for the containing element, otherwise it collapses - can set it to height: 100% 


### Fixed positioning

* A fixed position element is positioned relative to the viewport, or the browser window itself. The viewport doesn't change when the window is scrolled, so a fixed positioned element will stay right where it is when the page is scrolled
* also taken out of normal document flow - as does not affect position of other elements - so may also need to add padding-top to the body or content of the page below the fixed element eg:
```
body {
  padding-top: 100px;
}
```

#### Points to remember
* Elements positioned absolute have a higher z-index than fixed elements by default, but as you will often want the absolute position elements to scroll under the fixed position elements, you may need to give the fixed elements a higher z-index


#### Absolute centering
```
.icon {
	background-color: #39add1;
	margin-top: 34px;
	height: 180px;
	border-radius: 5px;
	position: relative;
}
.icon::after {
	content: "";
	display: block;
	width: 150px;
	height: 90px;
	background: url('../img/icon.png') no-repeat;
	background-size: 100%;
	position: absolute;
	top: 0;
	right: 0;
	bottom: 0;
	left: 0;
	margin: auto;
}
```


## FLEXBOX

* in order to use flexbox we'll need to establish a flex formatting context:
```
.main-nav {
  display: flex;
  height: 100%;
}
```

* display flex property makes the list items inside main nav flex items, which by default lays everything out in a row - this is called the flex direction
```
.main-nav li {
  align-self: center;
  flex-grow: 1;
  margin-left: 8px;
  margin-right: 8px;
}
```

* can use flex-grow property to control the amount of space each item takes up in the main-nav
* for all links to take up same width and be even distributed in the header: flex-grow: 1; the value 1 represents the ratio of space we want the list items to take up - each items taking up the same amount of space in the header
```
.main-nav::first-child {
  flex-grow: 1.5;
}
```
--> so logo takes up 1.5 space of flexbox item

* in order to work in safari will need to add vendor prefix
```
.main-nav {
  display: -webkit-flex;
}
.main-nav li {
  -webkit-align-self: center;
  align-self: center;
  -webkit-flex-grow: 1;
  flex-grow: 1;
}
```
[IE 10 syntax](https://msdn.microsoft.com/library/hh673531)

[align-self property](https://developer.mozilla.org/en-US/docs/Web/CSS/align-self)

[flex-grow property](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-grow)

/* Example of project using flexbox -
http://codepen.io/mborsare/pen/OybBJP */

#### Modenizr
* detects if a feature is supported by the browser and if it isn't
can customise for your purposes ie. testing if flexbox is supported
[https://modernizr.com/]

* will need to provide fallback styles for those that aren't supported
```
.main-nav li {
  margin: 0 8px;
  align-self: center;
  flex-grow: 1;
  transition: .5s;
}
  .no-flexbox .main-nav li {
    display: inline-block;
    margin-top: 12px;
    width: 150px;
  }
```
  

## GRID LAYOUTS

[resources](https://github.com/Guilh/G-Grid)

Pixel to percentage formula: target / context = result

e.g
```
.grid-1 {
  width: 6.5%;
}
/* 65px no gutter so 65/1000 (x100) = 6.5% */

.grid-2 {
  width: 15%;
}
* 65px + 65px + 20px for 1 gutter = 150px, then 150 / 1000 (total width of grid system) (x 100) = 15% */
  
.grid-12 {
  width: 100% * max-width */
}
```