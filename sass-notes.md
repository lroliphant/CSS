## Sass notes



### & 


```
.blog {
  > h1 {
    color: blue;
  }
  .entry {
    h1 {
      color: red;
    }
    p {
      margin: 20px;
    }
    html.csscolumns & {
      column-count: 2;
      column-gap: 10px;
      margin: 10px;
    }
    a {
      color: blue;
      &:hover {
        color: pink;
      }
    }
  }
}
```


### Variables

* Allow you to easily change one variable that is referenced throughout stylesheets easily
* Variables are placeholders for a value
* All variables start with a dollar sign

Example:
```
  $primary-colour
  $light_blue
```
  
Can do maths on variables e.g.
```
  $margin: 5px;
  $padding: $margin * 1.5;
  
  .entry {
    margin: $margin;
    padding: $padding;
  }
  h1 {
    padding: 5px / 2;
  }
```
  
### Mixins

* No point putting these everywhere, but if you find yourself using the same code 2/3 times then it is probably a good candidate for a mixin or variable
* only if you use it 6 or more times is it a good candidate for a global mixin


```
@mixin roundy {
  border-radius: 4px;
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
} 

@mixin green_links {
  a {
    color: green;
    &:hover {
      color: grey;
    }
  }
}

.box {
  @include roundy;
  @include green_links;
}
```

Passing arguments into mixins:
```
@mixin roundy($radius, $color) {
  border-radius: $radius;
  border-top-right-radius: $radius * 2;
  border-bottom-right-radius: $radius * 2;
  a {
    color: $color;
  }
} 

.box {
  @include roundy(4px, blue);
}

.button {
  @include roundy(2px, red);
}
```

### Extending selectors
 - @extend directive
 * allows you to extend the attributes from one class or id to another selector
 
 ```
 .bar {
   height: 14px;
   font-size: 10px;
   > div {
     float: left;
     clear: none;
   }
 }
 
 .menu {
   @extend .bar;
 }
 ```
 Using placeholder selectors (where bar is a placeholder selector but it not actually used or printed in the CSS):
 ```
%bar {
   height: 14px;
   font-size: 10px;
   > div {
     float: left;
     clear: none;
   }
 }
 
 .menu {
   @extend %bar;
 }
 
.nav {
  @extend %bar;
}
 ```
 
 When to use mixins vs extends?
 - mixings are great for arguments and advances logic etc (lots on that later)
 - extends are great if you have 2 attributes that you want copied, ie if you have some presentation classes like bar (see above)
 
 
### Colour functions
- enable you to create colour palettes

[http://sass-lang.com/documentation/file.SASS_REFERENCE.html#keyword_arguments]

[http://sass-lang.com/documentation/file.SASS_REFERENCE.html#color_operations]

```
$background: #cd3cc1;
$text-color: complement($background);
$highlighted-text-color: lighten($text-color, 20%);


body {
  background: $background;
  color: $text-color;
}

a {
  color: $highlighted-text-color;
}
```

### Importing files

[@import](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#import)
[Partials](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#partials)
[Nested @import] (http://sass-lang.com/documentation/file.SASS_REFERENCE.html#nested-import)

```
@import "_reset.scss";

// Defining things
@import "_variables.scss";
@import "_mixins.scss";

// Global styles
@import "_globals.scss";

// Page specifics
@import "pages/_about_us.scss"
```

- the underscore at the beginning of the file name means that Sass knows not to generate a separate css file for that scss file
- order of imported styles is important - for example the globals will come first and any page specific styles that override global styles will come next - cascade

##### Bourbon

[http://bourbon.io/]
[http://bourbon.io/docs/#linear-gradient]
[http://bourbon.io/docs/#font-family]


### Sass functions
```
$color: #369

@function pxify($value) {
  @return unquote($value + "px");
}

.red {
  background: red;
  width: pxify(red($color));
}
```
[Function directives](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#function_directives)


### Debugging Sass
* Whenever you see an error referenced at the end of the file - it usually means a curly brace is missing earlier in the file
* to see line number in sass that css is generated from - useful when using lots of files, extends and mixins - sometimes hard to know where something came from
```
sass -l main.scss:main.css
```
* [@debug](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#_4)


### Media Queries
* Add media queries inline in scss:
```
.content {
  .sidebar {
    background: #999;
    color: white
    padding: 5px;
    float: left;
    width: 100px;
    @media (max-width: 480px) {
      width: 95%;
    }
  }
}
```
[using @media directive](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#media)


### Interpolation and if/else statements
* interpolation - slotting values into other values

```
@mixin color_class($color) {
	.#{$color}.color {
		color: $color;
		background-image: url("images/#{$color}.jpg");
		@if $color == red {
			border: 1px solid black;
		}
	}
}

@include color_class(blue);
@include color_class(red);
@include color_class(green);
```

[@if and @else statements](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#_6)


### Creating loops with @for and @each

* @for loop is used when you want to iterate or go through numbers
[@for](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#_7)

* @each loop is used when ...
[@each](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#each-directive)

```
.box {
	width: 100%;
	height: 10px;
}

@each $member in thom, jonny, colin, phil {
	.bandmember.#{$member} {
		background: url("images/#{$member}.jpg")
	}
}

@for $i from 1 through 100 {
	.box:nth-child(#{$i}) {
		background: darken(white, $i);
	}
}

```

### Advanced Mixing Arguments

```
@mixin box($size: 10px, $color: black, $display: block) {
	width: $size;
	height: $size;
	background: $color;
	display: $display;
}

.box {
	@include box($display: inline);
}


@mixin band($name, $members...) {
	@each $member in $members {
		.#{$name}.#{$member} {
			background: url("images/#{$name}/#{$member}.jpg")
		}
	}
}

@include band(radiohead, thom, jonny, colin, phil);
@include band(nin, trent);
```

* can pass default values into mixin parameters

* [Passing Content Blocks to a Mixin](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#mixin-content)
* [Variable Scope and Content Blocks](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#variable_scope_and_content_blocks)
* [Keyword Arguments](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#keyword_arguments_2)
* [Variable Arguments](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#variable_arguments)


## Modular CSS w Sass

* [@if directive](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#_9)

* The @if directive takes a SassScript expression and uses the styles nested beneath it if the expression returns anything other than false or null:

```
p {
  @if 1 + 1 == 2 { border: 1px solid;  }
  @if 5 < 3      { border: 2px dotted; }
  @if null       { border: 3px double; }
}
```
is compiled to:
```
p {
  border: 1px solid; }
```


```
// variable-exists is a sass introspection function that returns whether a variable with a given name exists in our project
@if variable-exists(font-url) {
  // do something
  @import url($font-url--google);
}
```



### Px to em

* Commonly used function that converts px to em
```
// _utilities.scss
@function em($target, $context: $base__font-size) {
  @return ($target / $context) * 1em;
}
```

```
// _base.scss
body {
  font-size: $base__font-size;
  line-height: ($base__line/$base__font-size);
  font-family: $font-family--primary;
}

// using em function defined above
header {
  padding: em(48px) 0;
}

h1 {
  font-size: em(42px);
  line-height: (46px / 42px);
  margin-bottom: em(70px, 42px);
}
```


### Colours w Sass maps

* Sass maps can help to give our base colours a theme name - these theme names will come in handy later when we need o create classes for UI elements like buttons, form elements, progress bars, and more.
* Sass maps allow us to collet values in named grop by matching keys to values

* [Sass maps](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#maps)

* to retrieve values from maps we use key value pairs, e.g.
```
// _config.scss
$ui-colours: (
  default : $foundation-blue,
  success : $emerald,
  error   : $sunglo,
  warning : $coral,
  info    : $purple-majesty
);
```

* each directive to dynamically generate a class name for each key in our map and and outputs it's associated colour value as a background colour

* theme and colour variables represent the key, value pairs in the ui-colours map - so for each theme and colour in the ui-colours map, create this class selector for each key that dynamically appends the theme name with this theme variable, then pass its associated value as the background colour - outputting a css rule for each given theme name

```
@each $theme, $colour in $ui-colours {
  .btn--#{$theme} {
    background-color: $color;
  }
}
```

* wrap in a mixin to make the above more modular and reusable
```
// _utilities.scss
@mixin bg-colours($map) {
  @each $theme, $colour in $map {
    &--#{$theme} {
      background-color: $color;
    }
  }
}
```

```
// _main.scss
.btn,
.tooltip {
  @include bg-colours($ui-colours);
}
```

* creating shades and tones for some of our base colours using nested Sass maps:
```
// _config.scss

// colour palette modifiers
$palettes: (
  grey: (
    x-light :  lighten($grey, 35%),
    light   :  lighten($grey, 12%),
    base    :  $grey,
    dark    :  darken($grey, 8%),
    x-dark  :  darken($grey, 16%)
  ),
  black: (
    light : lighten($black, 10%),
    base  : $black,
    dark  : darken($black, 10%)
  )
);

// UI colours
$ui-colours: (
  default : $foundation-blue,
  success : $emerald,
  error   : $sunglo,
  warning : $coral,
  info    : $purple-majesty
);
```

* to use these colour values in our Sass rule we use the map get function - to return a value in a map based on the key passed in
```
// _utilities.scss

// call the colour palette modifiers

@function palette($palette, $shade: 'base') {
  @return map-get(map-get($palettes, $palette), $shade);
}

```
```
h1 {
  color: palette(grey, x-dark);
}
```

### Background image mixin

* helper mixin that makes it easier to define an element's background image, width, height, and display property:
```
```

* helper mixin for generating pseudo-element shapes:
```
//_config.scss

// Path to Assets

$path--rel : "../img";
```

```
// _utilities.scss

@mixin img-replace($img, $w, $h, $disp: block) {
  background-image: url('#{$path-rel}/#{$img}');
  background-repeat: no-repeat;
  width: $w;
  height: $h;
  display: $disp;
}
```

```
// _main.scss

.site-logo {
  @include img-replace("logo.svg", 50px, 35px, inline-block );
}
```

* create pseudo placeholder for some common properties and values
```
// _helpers.scss

%pseudos {
  display: block;
  content: '';
  position: absolute;
}
```

* mixin for generating pseudo element shapes, specifically a toggle menu icon using the :before and :after pseudo elements:
```
// _utilities.scss

@mixin p-element($element, $element-w: null, $element-h: null) {
  &:#{$element} {
    @extend %pseudos;
    width: $element-w;
    height: $element-h;
    @content;
  }
}

```

* [Modular Pseudo-Elements with Sass](http://blog.teamtreehouse.com/modular-pseudo-elements-sass)
* [Smarter Sass Mixins with Null](http://blog.teamtreehouse.com/smarter-sass-mixins-null)


### Pseudo-Element Mixin with @error
 * build the toggle icon using our new pseudo-element mixinabove and also learn how the new @error directive is useful for displaying error messages in mixins and functions. 

```
<nav role="navigation">
  <span class="icn-toggle>
    <b class="screen-reader-txt">Toggle</b>
  </span>
</nav>
```

```
// _main.scss

.icn-toggle {
  @include p-element(before, 25px, 3px) {
    background: palette(grey, light);
    top: 4px;
  }
  width: 25px;
  height: 17px;
  position: absolute;
  border-top: solid 3px palette(grey);
  border-bottom: solid 3px palette(grey);
}
```

* give mixin a failsafe feature using Sass's @if and @warn/@error (latter for newer versions of Sass) directives, so css is only outputted if $element is a before or after pseudo element
```
// _utilities.scss

@mixin p-element($element, $element-w: null, $element-h: null) {
  
  @if $element == "before" or $element == "after" {
    &:#{$element} {
      @extend %pseudos;
      width: $element-w;
      height: $element-h;
      @content;
    }
  }
  @else {
    @error "'#{$element}' is not a valide psuedo element.";
  }
}

```



to get sass to watch for changes - type the following into command line:
```
sass --watch scss:css
```














#### Simplying Sass with ampersands

* In Sass, & references the parent selector of a declaration block, allowing the selector to be referenced from within itself
