## CSS

#### How CSS is parsed

Step 1: Resolve conflicting CSS declaration by looking at importance, specificity and then source order.

Importance order

1. User !importance declarations
2. Author !importance declarations
3. Author declarations
4. User declarations
5. Default browser declarations

Specificity

We calculate an element's selector specificity to determine which style we should use.

```
Inline | IDs | Classes | Elements
   0      0       0         0
```

Step 2: Process final CSS values

```
% (fonts)                - parent's computed font-size
% (lengths)              - parent's computed width
em (font)                - parent's computed font-size
em (lengths)             - current element's computed font-size
rem (fonts and lengths)  - root's computed font-size (16px)
vh                       - viewport height
vw                       - viewport width
```

It's important to remember that children elements inherits __computed values__ instead of declared values.

#### Rem

We should always use `rem` to declare font size, so we can easily make our font responsive.

```
/* 
  reset root font size, we want the default font size to be 10px
  so 10px / 16px (default root font size) = 62.5%
*/
html {
  font-size: 62.5%;
}
```


#### Position an absolute element in the center of the screen

The `top` and `left` position the element based on its parent container, while the `transform: translate(x, y)` positions the element based on the computed width or height itself.

```css
.text-box {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

#### Clip an element

The `clip-path` property specifies a specific region of an element to display.

```css
// clip a rectangle
clip-path: polygon(0 0, 100% 0, 100% 80%, 0 100%);

// clip a triangle
clip-path: polygon(50% 0, 100% 100%, 0 100%);
```

[Clippy](https://bennettfeely.com/clippy/) is a good tool to play around `clip-path` property.

#### Gradient background image

We can use gradient background on top of an image to achieve a gradient background effect.

```css
background-image: linear-gradient(
    to right bottom, 
    rgba(127, 212, 112, 0.8), 
    rgba(40, 180, 133, 0.8)), 
  url("hero.jpg");
```

#### Make an element to have hover and active pseudo style only when it is not disabled

```css
button:hover:enabled {
    /* your styles */
}

button:active:enabled {
    /* your styles */
}
```

#### UI layer

z-index property specifies the z-order of a positioned element. When elements overlap, z-order determines which one sits on top of the other. An element with a larger z-index stacks over one with a lower z-index. Following is a recommended UI layer standard for web/mobile design.

```
Popup Layer       -> for displaying a popup
================
Mask Layer        -> for displaying a mask, locking navigation and content layer
================
Navigation Layer  -> for displaying navigation, it fixes on the position when the content scrolls
================
Content Layer     -> for displaying content
================
```

#### BEM

Block(.) - standalone entity that is meaningful on its own

Element(__) - a part of a block that has no standalone meaning and is semantically tied to its block

Modifier(--) - a flag on a block or element to change appearance or behavior

```css
// prefix-block__element--modifier
.eh-toolbar__arrow-btn--selected
```

#### Mobile first design

Mobile first design should write CSS for mobile screen first then use `min-width` media queries to write styles for larger screens.

```css
// This applies from 0px to 600px
body {
  background: red;
}

// This applies for larger screen from 600px onwards
@media (min-width: 600px) {
  body {
    background: green;
  }
}
```

#### Add animation to box-shadow

```css
.card {
  box-shadow: 0 0 2px 0 rgba(119, 119, 119, 0.3);
  // transition property is the key for animation
  transition: box-shadow 0.5s;
}

.card:hover {
  /* Any number of shadows, separated by commas */
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
}
```

#### Style children elements when hover on a parent

This can be achieved by using the `:hover` selector on the parent element.

```css
// change the color of .child-element when you hover .parent-element
.parent-element:hover .child-element {
  color: #3ec7de;
}
```

#### Create a drop caps effect

The drop caps effect is achieved using the `:first-letter` pseudo class. Basically what you need to do is to select the first letter and make it huge and floated to the left.

```css
p:first-child:first-letter {
  color: #903;
  float: left;
  font-family: Georgia;
  font-size: 75px;
  line-height: 60px;
  padding-top: 4px;
  padding-right: 8px;
  padding-left: 3px;
}
```

#### Pure CSS triangle

The triangle effect is achieved by leveraging the border property. Below is a series of steps to help you remember this trick.

1. Image a box element with 4 thick borders
2. Notice how the borders meet each other at angles
3. Make the box element's height and width to zero, so only the four borders are visible now
4. Make three of the borders transparent in color, so only one border in the shape of a triangle left

Hooray, this is how a triangle is made in CSS!

#### Parallax scrolling effect

The key to parallax scrolling effect is to add `background-attachment: fixed` property.

1. Create a background image container
2. Apply the following class to the container

```css
.parallax {
  /* The image used */
  background-image: url("bg.jpg");

  /* Set a specific height */
  min-height: 500px; 

  /* Create the parallax scrolling effect */
  background-attachment: fixed;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
```

#### resetting or normalizing CSS

Resetting destroys all the built-in styling while normalizing just tries to make built-in styling consistent across browsers. Choosing one over the other depends on what you want to achieve, but most of the time normalizing should be the one to go with.

Nowadays, different browsers behave more or less the same, so resetting or normalizing CSS through a third party library is no longer a necessity. We can rely on universal selector `*` to reset a few properties instead.

For anything related to font, it should set in the body element as this can take advantage of the CSS inheritance.

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Lato", sans-serif;
  font-weight: 400;
  font-size: 16px;
  line-height: 1.7;
}
```

#### CSS floats and clearfix hack

When you apply float to an element, it basically pulls it out from the normal document flow so it gets on top of the document flow. Since a floated element does not stay in the document flow, a container would not detect its existence, which leads to the classic `zero height collapse` container issue. To solve this, what we need is a clearfix hack.

```css
.clear:after {
  clear: both;
  content: "";
  display: table;
}
```
