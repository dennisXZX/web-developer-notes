## Javascript

#### + and , in console.log()

Using `+` and `,` in console.log() would result in different results.

```
// trying to concatenate an object will call toString() on it, resulting in [object Object] 
// output: App store created: [object Object]
console.log('App store created: ' +  appStore.getState());

// using comma actually means to pass another argument to console.log
// output: App store created:  { users: {}, items: [] }
console.log('App store created: ',  appStore.getState());
```

#### asyn and await

asyn and await syntax is an improved way to handle asynchronous code in Javascript. 

Instead of using a chain of `.then()` method, now we can handle asynchronous code in a more synchronous way.

```
function fetchAlbums() {
  fetch('http://rallycoding.herokuapp.com/api/music_albums')
    .then(res => res.json())
    .then(json => console.log(json));
}
```

```
async function fetchAlbums() {
  const res = await fetch('http://rallycoding.herokuapp.com/api/music_albums');
  const json = await res.json();
  console.log(json);
}
```

#### What is a shim and a polyfill?

A shim intercepts existing API calls and implements different behaviors in order to enhance backward compability.

A polyfill is code that detects if a certain "expected" API is missing and manually implements it.

#### How to enforce a maximum arity (number of passed parameters) for a function?

The ES6 rest operator is a handy way to handle parameters, which completely replaces the use of `arguments` array-like object.

Solution 1
```
function max(...values) {
    if (values.length > 3) {
        throw Error('max 3 parameters')
    }
    
    let [a, b, c] = values;
    return Math.max(a, b, c);
}
```

Solution 2
```
function max(a, b, c, ...shouldBeEmpty) {
    if (shouldBeEmpty.length > 0)
        throw Error('max 3 parameters allowed!');
 
    return Math.max(a, b, c);
};
```

#### How to clean up the signature of a function that accepts a large number of parameters?

Sometimes we need to pass a large number of parameters to a function, which is inconvenient and error-prone.
```
function addPerson(first, last, dob, gender, address) {
  // code
}
```
We can simplify that by introducing a configuraton object.
```
// wrap all parameters into a configuration object
function addPerson(config) {
  // code
}

const config = {
  username: 'dennisboys',
  first: 'dennis',
  last: 'xiao'
}

addPerson(config);
```

#### How to take advantage of function properties to cache computed results?

Function properties is usually used in memorization pattern, caching the results of a function.

```
let myFunc = function(param) {
  // if there is no cache in the function, calculate the result
  if (!myFunc[param]) {
    let result = 0;
    // expensive operation
    // result = operation result
    myFunc[param] = result;
  }
  return myFunc[param];
};
```

#### What are the differences between for-in and for-of operator. What are the benefits of using for-of over forEach?

The for-in operator is used to iterate over keys of objects.
The for-of operator is used to iterate over values of arrays or other iterables (Map, Set, String or DOM collection).

The benefits of for-of over forEach() is that we can use `break`, `continue`, and `return` in the loop which are not allowed to do in a forEach() call. All in all, the ES6 for-of operator introduces more flexibility when iterating over an array.

#### How to check if a value is an array before ECMAScript 5

Array.isArray() is introduced in ECMAScript 5 to determine if a value is an array. Before that, we have to rely on the method of `toString()` to find out the answer.

```
if (typeof Array.isArray === "undefined") {
  Array.isArray = function(arg) {
    return Object.prototype.toString.call(arg) === "[object Array]";
  };
}
```

#### How to enforce a function is always called as a constructor?

The following self-invoking constructor pattern can ensure a function is always called as a constructor.
```
function Person() {
  // check whether this is an instance of your constructor
  if (!(this instanceof Person)) {
    return new Person();
  }
  this.name = "Dennis";
}

Person.prototype.sayName = function() {
  return this.name;
}
```

#### What happen under the hood when you invoke a constructor function with a 'new' keyword?

- An empty object is created and referenced by `this` variable, which inherits the prototype object of the function.
- Properties and methods are added to the empty object.
- The newly created object is returned at the end implicitly (if no other object is return explicitly).

#### How to access a global object in all environments?

By returning `this` inside a function, `this` should always point to the global object.
```
const global = (function() {
  return this;
})();
```

#### How to declare variables in single var/let/const pattern?
```
let a = 1,
    b = 2,
    sum = a + b
```

#### In JavaScript, what's the difference between this, $(this) and $this?

‘this’ is a Javascript keyword. The value of ‘this’ varies depending on how a function is invoked. Mainly there are four different patterns.

- when invoked as a function, ‘this’ refers to the window object in non-strict mode, or `undefined` in strict mode. 
- when invoked as a method, ‘this’ refers to the object that calls the method.
- when invoked as a constructor function, ‘this’ refers to the object instance created by the constructor.
- when invoked by using call() and apply() method, ‘this’ refers to the object passed in as the first parameter. If the first parameter is `null`, 'this' would point to the global object.

In ES6, an arrow function does not create its own context, so 'this' has its original context from the enclosing scope.

$(this) is not a legitimate Javascript variable. In jQuery library, however, it means to construct a jQuery object so you can call jQuery methods on the object.

$this is a legitimate Javascript variable.

#### Explain event delegation

The concept behind this fancy term is actually quite simple. Event delegation simply means you can attach an event listener to a parent element, then events happen in its children will also trigger the event thanks to the event bubbling or event propagation.

#### Explain how `this` works in JavaScript

The value of 'this' is determined by how a function is called.

There are usually four ways of calling a function in Javascript.

1. When called as a function, 'this' refers to the global object, which is the window object in a browser environment (non-strict mode), or `undefined` (strict mode).
2. When called as a method, 'this' refers to the object that invokes the method.
3. When called as a constructor function, 'this' refers to the instance created by the constructor function.
4. When called via call() or apply(), 'this' refers to the first parameter passed into those functions. If the first parameter is `null`, then 'this' would point to the global object.

In ES6, an arrow function does not create its own context, so 'this' inherits the function context from the context in which it was created.

Take a look at this example to understand how arrow function affects the value of 'this'.

```
function Ninja() {
  // the value of 'this' is defined when a new Ninja object is created 
  this.whoAmI = () => this;
}

var ninja1 = new Ninja();

var ninja2 = {
  whoAmI: ninja1.whoAmI
};

// pass
assert(ninja1.whoAmI() === ninja1, "ninja1 here?");

// fail, ninja2.whoAmI() should be equal to ninja1
// because ninja2.whoAmI refers to ninja1.whoAMI, the context of 'this' is defined when a new Ninja object ninja1 is created, so 'this' will always refer to ninja1.
assert(ninja2.whoAmI() === ninja2, "ninja2 here?");
```

#### Explain how prototypal inheritance works

Though Javascript has introduced a new 'class' keyword in ES6, but, under the hood, it still achieves inheritance through prototype.

In Javascript, function is nothing but object, and each object has a 'prototype' object attached to it. Everything in the 'prototype' object is inherited by the instances of that object.

For detailed explanation about Javascript inheritance, I have previously written [a blog post](https://dennisboys.github.io/How-Prototypes-Work/ "How Prototypes Work") about it.

#### What do you think of AMD vs CommonJS?

Both AMD and CommonJS are specifications on how modules and their dependencies should be declared in Javascript applications. AMD loads modules asynchronously while CommonJS works synchronously.

#### Explain why the following doesn't work as an IIFE: `function foo(){ }();`.

Because it will be treated as a function declaration instead of a function expression. Any statement begins with a 'function' keyword will be treated by the Javascript parser as a function declaration. To make this IIFE works, you need to wrap the function with a bracket.

```  
(function foo(){})() or (function foo(){}())
```

#### What's the difference between a variable that is: `null`, `undefined` or undeclared? How would you go about checking for any of these states?

- 'undeclared' means a variable is not declared with var, let or const keyword
```
a = 0;
```
- 'undefined' represents a variable declared but not assigned
```
var b;
```
- 'null' represents an intentional absence of a value
```
var c = null;
```

You can use strict equality comparison operator '===' to check the above states.

```
if (value === undefined) || (value === null)
```

#### What is a closure, and how/why would you use one?

Closure is when a function can remember and access its lexical scope even when it's invoked outside its lexical scope.

Closure is everywhere. It can be used to protect private variables or internal functions, for example, it can be used in pattern like module. Also, when you are using a callback function, chances are you have already run into closure.

#### What's a typical use case for anonymous functions?

Anonymous function can be used in callback function, as it is called by a function instead of by you, so it can go without a function name. Another typical usage of anonymous function is Inmediately Invoked Function Expression (IIFE), as it is invoked the moment defined, so a function name is not necessary. 

#### How do you organize your code? (module pattern, classical inheritance?)

Now I have turned to ES6 modules to organize my code.

#### What's the difference between host objects and native objects?

Host objects are the objects given to you by the environment. Javascript can run on different environments, such as on a browser or a server. Native objects are the objects given to you by Javascript. You will get the same native objects no matter where you run your Javascript code, but host objects will be different depending on the running environment.

#### Difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?

- function Person(){} declares a constructor function.
- var person = Person() declares a person variable which holds the value returned from calling Person().
- var person = new Person() declares a person variable which holds an instance of the Person object.

#### What's the difference between `.call` and `.apply`?

Both call() and apply() can be used to alter the 'this' context of a function. The difference between them is how they accept parameters. call() accepts parameters one by one explicitly, while apply() accepts an array as its parameter.

#### Explain `Function.prototype.bind`.

Function.prototype.bind is a function defined in the prototype object of Function constructor, which means all instances of Function can access to bind() via prototypal inheritance. The bind function accepts a context as a parameter and returns a function that binds the context to its 'this' keyword. The 'this' context cannot be changed after the binding.

#### When would you use `document.write()`?

In no situation I find myself have a need for document.write(). It was used in the past to inject third party script into a web page, but this approach has been frowned upon due to the performance issue caused.

#### What's the difference between feature detection, feature inference, and using the UA string?

Feature detection is a way of determining if a feature exists in certain browsers.

```
if (navigator.geolocation) {
  // do something with geolocation
}
```

Feature inference is to assume whether a browser has certain features based on the testing results of another feature. This practice is always frowned upon as it attempts to use multiple features after validating the presence of only one. This might lead to unforeseen issues in the future.

UA string is short for User Agent string, which is a string each browser sends and can be accessed via navigator.userAgent. This string contains information of the browser environment you are targeting.

Luckily, you can use [Modernizr](https://modernizr.com/) library to do feature detection easily.

#### Explain Ajax in as much detail as possible.

Simply put, Ajax is the use of JavaScript to send and receive data using HTTP without refreshing a web page. It is used to make the browsing experience smoother by dynamically updating the content on a web page.

The process of execution of Ajax looks something like the following:

1. A user interaction in a browser triggers an event, such as a button click. 

2. An XMLHttpRequest object is created by Javascript and sent to a web server.

3. The server sends back a response after processing the request from the browser.

4. The response is handled by Javascript in the browser to perform appropriate actions.

#### What are the advantages and disadvantages of using Ajax?

Advantages: 

1. User experience is much better as no full page reload is required.
2. Save bandwidth because only part of the page is dynamically updated.

Disadvantages:

1. No browser history is registered for the new state, so it's impossible to use Back and Forward button to navigate between various states of a page.
2. User cannot bookmark a specific state of a web page.
3. Data loaded through AJjax won't be indexed by search engines.
4. The page breaks with Javascript is disabled.

#### Explain Cross-Origin Resource Sharing (CORS)

CORS is a mechanism that allows you to work around the same-orign policy implemented by browsers. By enabling CORS on your server, you sepcify what other servers can have access to your resources. Therefore, your server would respond to requests with an Access-Control-Allow-Origin header to let the browser know if the requested resource is accessible to those origins.

#### Explain how JSONP works

JSONP stands for JSON with Padding, yet another poorly named term in the programming field. It is a technique to address the same domain policy implemented in the browser land, which relies on `<script>` tags to bypass the restriction.

Say you send a request http://www.example.net/sample.aspx?callback=mycallback to a JSONP enabled server, it will then return a result wrapped in the callback function you specified. Normally the word 'callback' is used as a string query to tell the server it is a JSONP request.

Without JSONP, the returned result would be a run-of-the-mill JSON format.
```
{ foo: "happy coder" }
```
With JSONP, the returned result would be wrapped in a function.
```
mycallback({ foo: "happy coder" });
```
So, in your program, you can define a callback function name 'mycallback' to handle the response:
```
mycallback = function(data) {
  alert(data.foo);
};
```

#### Have you ever used JavaScript templating?
  
I used EJS before. EJS is a simple light-weight templating language that goes well with Express framework. Lodash library also has provided a convenient template feature.
  
#### Explain "hoisting".

To put it simply, when you declare a variable or a function, it will be hoisted (magically) to the top of the scope (global scope or function scope). So you can use the variable or call the function even before its declaration. It is important to note that the hoisting applies only to function declaration, not function expression.

But the above statement glosses over a lot of details. The 'hoisting' is actually caused by the way how Javascript engines works. Javascript code interpretation is performed in two phases. During the first phase, the interpreter parses variable and function declarations and put them in memory. This phrase is commonly known as 'hoisting'. In phase two, Javascript engine starts to execute the code. Since variable and function declarations have already be placed in memory in the parsing phrase, that is why we can invoke them in our code before their declarations.

#### Describe event bubbling (event propagation).

To put it simply, when an event happens on an element, it first runs the handlers on it, then on its parent, then all the way up on other ancestors.

When an event happens, it is possible to capture where exactly it happens by accessing the `event.target` property. It is important to note `event.target` is not equal to `this`, which refers to an element that registers the triggered event handler.

You can use `event.stopPropagation()` to stop the bubbling, but normally there is no need to do so.

[Here is an article that clearly explains the bubbling concept](http://javascript.info/bubbling-and-capturing "How Bubbling Works")

#### What's the difference between an "attribute" and a "property"?

In Javascript, an object can have as many properties as you want. For example, the following object has two properties, name and age.
```
let obj = {
  name: 'Dennis',
  age: 34
}
```
Each property of an object has a few built-in attributes, such as `configurable`, `enumerable` and `writable`, etc. Most of the time you don't want to touch these attributes, but in special occasions, you can alter these attributes by calling `Object.defineProperties()` method.

#### Why is extending built-in JavaScript objects not a good idea?

Because when you extend a built-in Javascript object, you change its behavior and that poses a risk to other coders. People who use Javascript native objects would expect they behave the same every where. Therefore, you might inject some surprised moments into their lives by extending built-in objects. 

#### Difference between document load event and document DOMContentLoaded event?

The DOMContentLoaded event is triggered when all the HTML document has been completely loaded. The document load event, on the other hand, is triggered when all the HTML document and its resources (images, styles, etc) have been fully loaded. Therefore, DOMContentLoaded event is triggered before the document load event.

#### What is the difference between `==` and `===`?

The equality operator == will do a type conversion before comparing the two values, while the strictly equality operator === will just compare two values without doing any type conversion. It is highly recommended to use === in development to minimize any risks of unwanted type conversions.

#### Explain the same-origin policy with regards to JavaScript.

The same-origin policy restricts how a script loaded from one origin can interact with a resource from another origin. Two pages have the same origin if the protocol, port (if one is specified), and host are the same for both pages. To bypass the same-origin policy, we have to use [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS).

#### Make this work:
duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]

Use the ES6 spread operator to achieve the duplication.

```
function duplicate(arr) {
  return [...arr, ...arr];
}
```

#### Why is it called a Ternary expression, what does the word "Ternary" indicate?

Ternary expression is just a shortcut for if statement. The name ternary indicates it needs three parameters.

#### What is `"use strict";`? what are the advantages and disadvantages to using it?

'use strict' is used to enable strict mode for Javascript. Basically it is used to elimiate some Javascript quirks. For example, in strict mode, the 'this' keyword in a function invocation refers to `undefined` instead of a global `window` object. Also, the implicit `arguments` parameter in a function does not alias declared function parameters anymore.

#### Create a for loop that iterates up to `100` while outputting **"fizz"** at multiples of `3`, **"buzz"** at multiples of `5` and **"fizzbuzz"** at multiples of `3` and `5`

Non-function solution
```
for (let i=0; i< 101; i++) {
  const isFizz = i % 3 === 0,
        isBuzz = i % 5 === 0;

  let result = '';

  if (isFizz === 0 && isBuzz === 0) {
    result = 'FizzBuzz';
  } else if (isFizz === 0) {
    result = 'Fizz';
  } else if (isBuzz === 0) {
    result = 'Buzz';
  } else {
    result = i;
  }

  console.log(result);
}
```

Functional solution using the ternary operator
```
for (let i=0; i< 101; i++) {
  const isFizz = i % 3 === 0,
        isBuzz = i % 5 === 0;

  const result = 
    isFizz && isBuzz ? 'FizzBuzz'
      : isFizz ? 'Fizz'
        : isBuzz ? 'Fuzz'
          : i;

  console.log(result);
}
```

#### Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?

1. Hard to maintain if you put code in the global scope.
2. Any function can change a global variable at any point in the program.
3. Namespace clashes may happen in the global scope.

So it is a best pratice to keep the global scope as clean as you can.
