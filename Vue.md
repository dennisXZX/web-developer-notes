## Vue

#### Life cycle methods

Some important life cycle methods `mounted`, `updated` and `destroyed`.

```
const vm = new Vue({
  data: {
    a: 1
  },
  mounted: function () {
    // `this` points to the vm instance
    console.log('a is: ' + this.a)
  }
})
```

#### Vue instance

A Vue application consists of a root Vue instance and a tree of nested components.

```js
// create a root Vue instance
const vm = new Vue({

  // define the root element for the Vue
  el: '#app',
  
  // model to hold app data
  data: {
    message: 'Hello Vue World',
    intro: 'Welcome to my world',
    viewed: false,
    todos: [
      { text: 'Learn Vue', id: 1 },
      { text: 'Get Milk', id: 2 },
      { text: 'Buy Egg', id: 3 },
    ]
  },
  
  // computed property is cached, it only changes when its dependencies change
  computed: {
    fullName: {
      // getter
      get: function () {
        return `${this.firstName} ${this.lastName}`
      },
      // setter
      set: function (value) {
        const name = value.split(' ');
        this.firstName = name[0];
        this.lastName = name[name.length - 1];
      }
    }
  },
  
  // watch any change happens to the value
  watch: {
    message: function(value) {
      // store the 'this' reference, because in the callback function we need to access the data object on the Vue instance
      const vm = this;

      // reset the message 2 seconds later after the message value is changed
      // the value argument in the callback function is value of the upcoming change
      setTimeout(function(value) {
        vm.message = "";
      }, 2000);
    }
  }
  
  methods: {
    countUp: function () {
      // we can access any properties inside the data object using this.propertyName
      this.count += 1;
    },
    reset: function () {
      this.count = 0;
    }
  },
  
  filters: {
    capitalize: function(value) {
      if (!value) return '';
      value = value.toString();
      return value.charAt(0).toUpperCase() + value.slice(1);
    },
  }
  
})
```

#### Create a Vue component

```
// create a new component called todo-item
Vue.component('todo-item', {
  props: ['todo'],
  template: '<div>{{ todo.text }}</div>'
})
```

#### Vue instance properties and methods

```js
vm.$data retrieves the data property of the Vue instance

vm.$el retrieves the element of the HTML element specified in the 'el' property

vm.$watch('message', function(newValue, oldValue) {
  // This callback will be called when `vm.message` changes
}
```

#### Passing data from parent to child component

```js
// the 'todo-item' component accepts a prop of 'todo'
Vue.component('todo-item', {
  props: ['todo'],
  template: '<div>{{ todo.text }}</div>'
})
```

The parent component passes down the prop.

```
<div id="app">
  <p v-on:click="reverseMessage">{{ message }}</p>
  <todo-item 
    v-for="todo in todos"
    v-bind:todo="todo"  // pass the prop to child component
    v-bind:key="todo.text">
  </todo-item>
</div>
```

#### Directive

__v-for__

```js
// use v-for to loop through an array and print out its content
<li v-for="todo in todos">{{todo.id}}) {{todo.text}}</li>

// use v-for to execute a certain amount of times, the second 'count' is an integer, such as 10
<li v-for="count in count">#{{count}}</li>
```

```js
<button 
  // myStyle is a computed property which returns an object
  // based on the 'size' property, if it's bigger than 10 returns 'large' class
  // renders 'rounded' class if 'isRounded' property is true
  v-bind:class="[myStyle, size > 10 ? 'large' : '', {'rounded': isRounded}]"
  // v-bind:style is often used in conjunction with computed properties that return objects
  v-bind:style="styles"
  v-bind:disabled="disabled">
  Press Me!
</button>

// bind data to a property
<img v-bind:url="url" v-bind:alt="intro" v-bind:title="message"></img>
<a v-bind:href="link">Google</a>

// short-cut syntax for v-bind
<img :url="url" :alt="intro"></img>

// v-model for two-way binding
// when the 'message' property is changed the input text will be updated and vice verse
<input type="text" v-model="message">

// display the text in the element
<div v-text="message">hello</div>

// insert the HTML markup into the element
// WARNING: this might expose your site to XSS (Cross-site Scripting) if the HTML content inserted is not properly sanitized 
<div v-html="intro"></div>

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

#### Events

```js
// execute 'reset' method when the button is clicked
<button v-on:click="reset" class="btn btn-primary">Reset Me!</button>

// short-cut syntax for v-on
<button @click="reset" class="btn btn-primary">Reset Me!</button>
```

Passing own argument to an event, the `$event` argument is the original DOM event.

```html
<button v-on:click="clicked($event, 2)">Click</button>
```

```js
new Vue({
  el: '#app',
  data: {
    counter: 0
  },
  methods: {
    clicked: function(event, step) {
    	this.counter += step;
    }
  }
})
```

You may run into situations where you want to stop event propagation or prevent default event behaviors. You can easily achieve this by using `event modifiers`.

```html
<!-- the click event's propagation will be stopped -->
<a v-on:click.stop="doThis"></a>

<!-- the submit event will no longer reload the page -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- modifiers can be chained -->
<a v-on:click.stop.prevent="doThat"></a>
```

```html
<!-- only call `vm.submit()` when user hits enter -->
<input v-on:keyup.enter="submit">
```
