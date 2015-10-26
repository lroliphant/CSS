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

