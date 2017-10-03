## Vue

### Initialize Vue in your project

```
var app = new Vue({
  // define the root element for the Vue
  el: '#app',
  data: {
    message: 'Hello Vue World',
    intro: '<h1>Welcome to my world</h1>',
    viewed: false
  }
})
```

### Directive

```
// display the text in the element
<div v-text="message">hello</div>

// insert the HTML markup into the element
<div v-html="intro">hello</div>

// show or hide an element based on the boolean value (the HTML markup is still in the DOM)
<div v-show="viewed">hello</div>

// display or remove an element based on the boolean value (the HTML markup is not in the DOM if it's false)
<div v-if="viewed">hello</div>
<div v-else>hello</div>

// display the text as it is without trying to interpret
<div v-pre>{{hello if 2}}</div>

// render the component just once, so you cannot change its value after the first render
<div v-text="intro" v-once>hello</div>

// combine with CSS rules [v-cloak] { display: none } to hide uncompiled mustache bindings
<div v-cloak>{{ message }}</div>
```
