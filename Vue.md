## Vue

#### Life cycle methods

The are four different stages of a view - creation, mount, update and destroy. Some important life cycle methods are `created`, `mounted`, `updated` and `destroyed`.

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
    message: function(newValue, oldValue) {
      // reset the message 2 seconds later after the message value is changed
      setTimeout(() => {
        this.message = "";
      }, 2000);
    }
  }
  
  // if we bind an event like this - <div @mousemove="xCoordinate"></div>
  // the event object will be automatically passed to the method
  methods: {
    countUp: function () {
      // we can access any properties inside the data object using this.propertyName
      this.count += 1;
    },
    reset: function () {
      this.count = 0;
    },
    xCoordinate(e) { // the event object is passed automatically by Vue
      this.x = e.clientX;
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

We can change the data object from outside the Vue instance by storing the instance into an variable and then access its properties.

```js
const vm = new Vue({
  data: {
    title: 'test'
  }
})

// change the data object from outside
vm.title = 'new test';
```

#### Create a Vue component

Gloal component

```
// create a global component called todo-item
Vue.component('todo-item', {
  data() {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  props: ['todo'],
  template: '<div>{{ msg }}</div>'
})
```

Local component

```
// create a local component
const todo = {
  data() {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  props: ['todo'],
  template: '<div>{{ msg }}</div>'
}

// register it in a Vue instance
new Vue({
  el: '#app',
  components: {
    'todo': todo
  }
})
```

#### Slot

We can pass block of HTML content from parent to child by using slot.

In the parent template, we wrap the content inside the component tag.

```html
<app-quote>
  <h2>The Quote</h2>
  <p>A wonderful Quote</p>
</app-quote>
```

In the child template, we use the `<slot></slot>` tag to specify where the content should go. It is noted that the style should be defined in the child template instead of the parent one.

```html
<div>
  <slot></slot>
</div>
```

__named slot__

```html
<app-quote>
  <h2 slot="title">The Quote</h2>
</app-quote>
```

```html
<div>
  <slot name="title"></slot>
</div>
```

#### Dynamic components

If we want to dynamically change part of the view to a component, we can use the `<component>` tag.

```html
<!-- 'selectedComponent' need to be a string that matches a component name -->
<!-- by default, the component would be destroyed when you navigate away from it -->
<!-- you can use the <keep-alive> tag to change this default behavior -->
<!-- 'activated' and 'deactivated' are two life cycle methods that will be called when a component is toggled inside <keep-alive> -->
<keep-alive>
  <component :is="selectedComponent"></component>
</keep-alive>
```

```js
import Quote from './components/Quote.vue';
import News from './components/News.vue';

export default {
  data() {
    return {
      selectedComponent = "appQuote"
    }
  },
  components: {
    appQuote: Quote,
    appNews: News
  }
}
```

#### Vue instance properties and methods

`vm.$data` retrieves the data property of the Vue instance. You can also use `<pre>{{ $data }}</pre>` in the template to quickly debug the data object.

`vm.$el` retrieves the element of the HTML element specified in the 'el' property.

You can put a `ref` attribute to an easily refer to an DOM element. 

`<button @click="show" ref="myButton">Show</button>`

Then in your view model, you can refer to this button using `vm.$refs.myButton.innerText`.

```js
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

// props validation - String
Vue.component('child', {
  props: {
    text: {
      type: String,
      required: true,
      default: 'hello world'
    }
  },
  template: `<div>{{ text }}</div>`
})

// props validation - Object
// the default property need to be a function returning an object
Vue.component('child', {
  props: {
    text: {
      type: Object,
      required: true,
      default: function() {
        return { 
          message: 'hello world' 
        }
      }
    }
  },
  template: `<div>{{ text }}</div>`
})
```

```html
<!-- The HTML kebab-case property will be converted into camel case in the Vue component. -->

<!-- We bind `boolean-value` in the HTML. -->

<checkbox :boolean-value="booleanValue"></checkbox>

<!-- We would get props: ['booleanValue'] in the component -->
```

The parent component passes down the prop.

```html
<div id="app">
  <p v-on:click="reverseMessage">{{ message }}</p>
  <todo-item 
    v-for="todo in todos"
    :todo="todo"  // pass the prop to child component
    :key="todo.text">
  </todo-item>
</div>
```

#### Child component emit event to parent

If we need to report an event happen in the child component to its parent, we can use the `$emit`.

In the child component we emit an custom event.

```js
methods: {
  talkToMe() {
    this.text = 'new text';
    this.$emit('changeText', this.text); // emit an changeText event to its parent
  }
}
```

In the parent component we need to listen to the custom event.

```html
<!-- the $event is the value emitted from the changeText event, which is this.text in this case -->
<app-user-edit @changeText="name = $event"></app-user-edit>
```

#### Directive

__v-for__

```js
// use v-for to loop through an array and print out its content
<li v-for="todo in todos">{{todo.id}}) {{todo.text}}</li>

// use v-for to execute a certain amount of times, the second 'count' is an integer, such as 10
<li v-for="count in count">#{{count}}</li>

// you can retrieve the index of each array item
<div v-for="(post, i) in posts" class="post">
  <span class="label">{{ post.label }}</span>
  <p>{{ post.title }}</p>
  <small>{{ post.author }}</small>
</div>

// you can retrieve the value, key and index of each object
<div v-for="(value, key, index) in person">{{ key }}: {{ value }}</div>
```

__v-model and modifiers__

```js
// create a two-way binding
// when the 'message' property is changed the input text will be updated and vice verse
<input type="text" v-model="message">

<!-- You can also add modifiers to v-model -->

// synced after "change" event instead of "input" event
<input v-model.lazy="msg" >

// user input to be automatically typecast as a number
<input v-model.number="age" type="number">

// user input to be trimmed automatically
<input v-model.trim="msg">
```

__v-if and v-show__

```js
// show or hide an element based on the boolean value (the HTML markup is still in the DOM)
<div v-show="viewed">hello</div>

// display or remove an element based on the boolean value (the HTML markup is not in the DOM if it's false)
<div v-if="type === 'A'">hello</div>
<div v-else-if="type === 'B'">hi</div>
<div v-else>bye</div>

// we can also utilize <template></template> tag to group HTML tags
// the <template> tag would not be rendered in the DOM
<template v-if="show">
  <h2>Flower</h2>
  <img src="img_white_flower.jpg">
</template>
```

__v-bind__

```js
<button 
  // myStyle is a property which returns an object
  // based on the 'size' property, if it's bigger than 10 returns 'large' class
  // renders 'rounded' class if 'isRounded' property is true
  v-bind:class="[myStyle, size > 10 ? 'large' : '', {'rounded': isRounded}]"
  // v-bind:style is often used in conjunction with computed properties that return objects
  v-bind:style="styles"
  v-bind:disabled="disabled">
  Press Me!
</button>

// bind multiple styles
<div :style="{ width: playerHealth + '%', backgroundColor: playerHealthColor }">hello</div>

// bind multiple style objects, both accentColor and headers are an object containing CSS styles
<div :style="[accentColor, headers]">hello</div>

// mouseover event changes background color property using hue
<div id="app" :style="{ backgroundColor: `hsl(${colorValue}, 80%, 50%)` }" @mousemove="changeBg"></div>

// bind data to a property
<img :url="url" v-bind:alt="intro" v-bind:title="message"></img>
<a :href="link">Google</a>

// short-cut syntax for v-bind
<img :url="url" :alt="intro"></img>
```

__v-once and v-pre__

```js
// render the component just once, so you cannot change its value after the first render
<div v-text="intro" v-once>hello</div>

// display the text as it is without trying to interpret
<div v-pre>{{hello if 2}}</div>
```

__v-html__

```js
// insert the HTML markup into the element
// WARNING: this might expose your site to XSS (Cross-site Scripting) if the HTML content inserted is not properly sanitized 
<div v-html="intro"></div>
```

__v-cloak__

```js
// combine with CSS rules [v-cloak] { display: none } to hide uncompiled mustache bindings
<div v-cloak>{{ message }}</div>
```

#### Events and modifers

```html
// execute 'reset' method when the button is clicked
<button @:click="reset" class="btn btn-primary">Reset Me!</button>

// multiple bindings
<div v-on="
  click   : onClick,
  keyup   : onKeyup,
  keydown : onKeydown
"></div>
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
<!-- listen to native DOM events on a Vue component -->
<app-server-status @click.native="updateServer"></app-server-status>

<!-- the click event's propagation will be stopped -->
<a @click.stop="doThis"></a>

<!-- the submit event will no longer reload the page -->
<form @submit.prevent="onSubmit"></form>

<!-- modifiers can be chained -->
<a @click.stop.prevent="doThat"></a>
```

<!-- 
bypass event propagation all together, 
'stop' modifier to prevent event propagation, 
'self' modifier to ignore events that are not from the element itself 
-->
<button @click.stop.self="executeSearch">Stop Propagation</button>

__Key modifier__

```html
<!-- only call submit() when user hits enter -->
<input @keyup.enter="submit">
```

You can also define custom key modifiers by the global `Vue.config.keyCodes` object.

```js
// enable `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```
