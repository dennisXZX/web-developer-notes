## CSS Animation

#### Fundamentals

CSS animation is ofen used in combination with pseudo-class.

- `:hover` state indicates when you hover an element, like mouse hover
- `:focus` state indicates when an element receives focus, like click on an input field or use `tab` key moving to it
- `:active` state indicates when an element is being activated by user, like the moment you click on a button

Four things modern browsers can animate cheaply [`position, scale, rotation` and `opacity`](https://www.html5rocks.com/en/tutorials/speed/high-performance-animations/).

#### Keyframe animation

Check out lot of keyframe examples [here](http://animista.net/).

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
  animation: moveInLeft 0.5s ease-out 3s
  /* how many times should the animation repeat itself */
  animation-iteration-count: 3;
}
```

`animation-fill-mode` specifies a style for the element when the animation is not playing (before it starts, after it ends, or both).

`animation-fill-mode: backward`

The element will get the style values that is set by the first keyframe, and retain this during the animation-delay period.

`animation-fill-mode: forward`

The element will retain the style values that is set by the last keyframe.

`animation-direction: alternate` can be used to force an animation to an infinite loop.

`animation-play-state: paused` can be used to pause an animation (mostly use Javascript to change this property)

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

You can specify different `transition-duration` and `transition-delay` for different properties.

```css
.animated {
  transition-property: background, border-radius;
  transition-duration: 5s, 1s;
  /* delay the background transition for 0.4s, and delay the border-radius transitio for 1s */
  transition-delay: 0.4s, 1s;
}
```

`transition-timing-function` is used to control the acceleration curve for the transition.

[easings.net](https://easings.net/) is a great site to show you how `transition-timing-function` works.

[matthewlein.com/tools/ceaser](https://matthewlein.com/tools/ceaser) is a great tool to create easing animations.
