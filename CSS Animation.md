## CSS Animation

#### Keyframe animation

First, you define the how an animation should look like at each phase.

```css
@keyframes moveInLeft {
  0% {
    opacity: 0;
    transform: translateX(-100px);
  }

  100% {
    opacity: 1;
    transform: translate(0);
  }
}
```

Then you add this animation to an element.

```css
.heading-primary-main {
  animation-name: moveInLeft;
  animation-duration: 0.5s;
  /* how the animation should progress over the duration of each cycle */
  animation-timing-function: ease-out;
  /* how long do you want to delay the animation */
  animation-delay: 3s;
  /* shorthand writing */
  animation: moveInLeft 1s ease-out .75s
  /* how many times should the animation repeat itself */
  animation-iteration-count: 3;
}
```

`animation-fill-mode` specifies how a CSS animation should apply styles to its target before and after its execution.

`animation-fill-mode: backward`

The element will get the style values that is set by the first keyframe, and retain this during the animation-delay period.

`animation-fill-mode: forward`

The element will retain the style values that is set by the last keyframe.

`animation-direction: alternate` can be used to force an animation to an infinite loop.

#### Transition animation

Another simpler way to add animation to an element is by using the `transition` property.

```css
/* add transition property to the element that you want to animate */
.btn:link,
.btn:visited {
  text-transform: uppercase;
  text-decoration: none;
  padding: 15px 40px;
  display: inline-block;
  border-radius: 100px;
  transition: all .2s;  /* specify what property you would like the animation takes place */
}

/* specify the animation in other states of the element */
.btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, .2);
}

.btn:active {
  transform: translateY(-1px);
  box-shadow: 0 5px 10px rgba(0, 0, 0, .2);
}
```
