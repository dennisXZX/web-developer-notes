## CSS

#### Hover and active style only when an element is not disabled

```css
button:hover:enabled{
    /*your styles*/
}

button:active:enabled{
    /*your styles*/
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

#### Difference between classes and IDs

Most of us know that ID is unique while you can have multiple classes with the same name in a document. But there is a special feature for ID, which can act as an anchor. If you have a URL like dennisxiao.com#github, the browser will attempt to reach the section with an ID of 'github'.

#### resetting and normalizing CSS

Resetting destroys all the built-in styling while normalizing just tries to make built-in styling consistent across browsers. Choosing one over the other depends on what you want to achieve, but most of the time normalizing should be the one to go with.

#### CSS floats and clearfix hack

When you apply float to an element, it basically pulls it out from the normal document flow so it gets on top of the document flow. Since a floated element does not stay in the document flow, a container would not detect its existence, which leads to the classic `zero height collapse` container issue. To solve this, what we need is a clearfix hack.

```css
.clear:after {
  clear: both;
  content: "";
  display: table;
}
```
