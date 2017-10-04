## Vue

### Initialize Vue in your project

```
var app = new Vue({
  // define the root element for the Vue
  el: '#app',
  // model to hold app data
  data: {
    message: 'Hello Vue World',
    intro: '<h1>Welcome to my world</h1>',
    viewed: false,
    todos: [
      { text: 'Learn Vue', id: 1 },
      { text: 'Get Milk', id: 2 },
      { text: 'Buy Egg', id: 3 },
    ]
  },
  methods: {
    countUp: function () {
      this.count += 1;
    },
    reset: function () {
      this.count = 0;
    }
  }  
})
```

### Directive

```
// v-model for two-way binding
// when the 'message' property is changed the input text will be updated and vice verse
<input type="text" v-model="message">

// use v-for to loop through an array and print out its content
<li v-for="todo in todos">{{todo.id}}) {{todo.text}}</li>
// use v-for to execute a certain amount of times, the second 'count' is an integer
<li v-for="count in count">#{{count}}</li>

// bind data to a property
<img v-bind:url="url" v-bind:alt="intro"></img>
// short-cut syntax for v-bind
<img :url="url" :alt="intro"></img>

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

### Events

```
// execute 'reset' method when the button is clicked
<button v-on:click="reset" class="btn btn-primary">Reset Me!</button>
// short-cut syntax for v-on
<button @click="reset" class="btn btn-primary">Reset Me!</button>
```
