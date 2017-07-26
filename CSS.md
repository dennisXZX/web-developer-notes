## CSS

#### How to add animation to box-shadow?

.card {
    box-shadow: 0 0 2px 0 rgba(119, 119, 119, 0.3);
    transition: box-shadow 0.5s; // transition property is the key for animation
}
.card:hover {
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
}

#### How to style children elements when hover on a parent?

This can achieve by using the `:hover` selector.
```
.parent-element:hover .child-element {
  color: #3ec7de;
}
```

#### How to create a drop caps effect?

The drop caps effect is achieved using the `:first-letter` pseudo class. Basically what you need to do is to select the first letter and make it huge and floated to the left.

```
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

#### How to create a triangle?

The triangle effect is achieved by using the border property. Below is a series of steps to help you remember this trick.

1. Image a box element with 4 thick borders.
2. Notice how the borders meet each other at angles.
3. Make the box element's height and width to zero, so only the four borders are visible now.
4. Make three of the borders transparent in color, so only one border in the shape of a triangle left.
5. Hooray, this is how a triangle is made in CSS!

#### How to create a parallax scrolling effect?

The key to parallax scrolling effect is to set a background image `background-attachment: fixed`.

1. Create a background image container.
2. Apply the following class to the container. 

```
.parallax {
    /* The image used */
    background-image: url("1.jpg");
			
    /* Set a specific height */
    min-height: 500px; 

    /* Create the parallax scrolling effect */
    background-attachment: fixed;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
}
```

#### What is the difference between classes and IDs in CSS?

Most of us know that ID is unique while you can have multiple classes with the same name in a document. But there is a special feature for ID, which can act as an anchor. If you have a URL like dennisxiao.com#github, the browser will attempt to reach the section with an ID of 'github'.

#### What's the difference between "resetting" and "normalizing" CSS? Which would you choose, and why?

Resetting destroys all the built-in styling while normalizing just tries to make built-in styling consistent across browsers. Choosing one over the other depends on what you want to achieve, but most of the time normalizing should be the one to go with.

#### Describe Floats and how they work.

When you apply float to an element, it basically pulls it out from the normal document flow so it gets on top of the flow. Since a floated element does not stay in the document flow, a container will not detect its existence, which leads to the classic 'zero height' container issue. To solve this, what we need is a clearfix hack.

```
.clear:after {
    clear: both;
    content: "";
    display: table;
}
```

#### Describe z-index and how stacking context is formed.

z-index property specifies the z-order of a positioned element. When elements overlap, z-order determines which one covers the other. An element with a larger z-index stacks over one with a lower z-index.
