Notes to be a better web developer. Keep coding, keep smiling!

## Table of Contents

  1. [HTML](#html)
  1. [CSS](#css)
  1. [Javascript](#javascript)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Network](#network)
  1. [Database](#database)
  1. [Coding](#coding)

## HTML

#### What does a `doctype` do?

DOCTYPEs are required for legacy reasons. When omitted, browsers tend to use a different rendering mode that is incompatible with some specifications. Including the DOCTYPE in a document ensures that the browser makes a best-effort attempt at following the relevant specifications.

#### How to make a website accessible?

Here are few important things that need to be considered in making a website accessible.

- Use semantic HTML to structure your website.
- Always include alt text for images.
- Give links descriptive names.
- Ensure that all content can be accessed by keyboard.
- Use ARIA roles to enhance the semantics of your website.

#### What is microformats?

Microformats is a way to extend HTML markup by introducing some standardized class names for information they contain. Below is a hCard 

```
<p class="h-card">
  <img class="u-photo" src="http://example.org/photo.png" alt="" />
  <a class="p-name u-url" href="http://example.org">Joe Bloggs</a>
  <a class="u-email" href="mailto:joebloggs@example.com">joebloggs@example.com</a>, 
  <span class="p-street-address">17 Austerstræti</span>
  <span class="p-locality">Reykjavík</span>
  <span class="p-country-name">Iceland</span>
</p>
```

## CSS

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

## Javascript

#### In JavaScript, what's the difference between this, $(this) and $this?

‘this’ is a Javascript keyword. The value of ‘this’ varies depending on how a function is invoked. Mainly there are four different patterns.

- when invoked as a function, ‘this’ refers to the window object in non-strict mode, or `undefined` in strict mode. 
- when invoked as a method, ‘this’ refers to the object that calls the method.
- when invoked as a constructor function, ‘this’ refers to the object instance created by the constructor.
- when invoked by using call() and apply() method, ‘this’ refers to the object passed in as the first parameter.

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
4. When called via call() or apply(), 'this' refers to the first parameter passed into those functions.

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

```
function fizzbuzz() {
    for (let i=1; i<100; i++) {
        if (i % 3 && i % 5 == 0) {
            console.log(i + ': fizzbuzz');            
        } else {
            if (i % 3 == 0) {
                console.log(i + ': fizz');
            }
            if (i % 5 == 0) {
                console.log(i + ': buzz');
            }
        }
    }
}
```

#### Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?

1. Hard to maintain if you put code in the global scope.
2. Any function can change a global variable at any point in the program.
3. Namespace clashes may happen in the global scope.

So it is a best pratice to keep the global scope as clean as you can.

## Testing

## Performance

## Network

## Database

### Relational

### NoSQL

## Coding

#### What is the value of `foo`?

var foo = 10 + '20';

foo is a string with a value of '1020'. 

This is because when you try to concatenate a number with a string, the number will be automatically converted into a string before concatenation.

#### How would you make this work?

```
add(2, 5); // 7
add(2)(5); // 7
```
```
// answer
function add(num1, num2) {
  if(arguments.length === 1) {
    return function(num2) {
      return num1 + num2;
    }
  }
  return num1 + num2;
}
```
#### What value is returned from the following statement?
```
"i'm a lasagna hog".split("").reverse().join("");
```
After the method chain, the returned value is 'goh angasal a m'i'. 

First the string is split into an array of characters because the split() function is called passing an empty string as parameter. After that, the array is reversed then joined together.

#### What is the value of `window.foo`?
```
( window.foo || ( window.foo = "bar" ) );
```
The value of window.foo is 'bar'.

This expression first evaluates the left hand side of the || operator, which is a property retrieval expression that produces an `undefined` value. Then it evaluates the right hand side, which is an assignment expression that assigns a string 'bar' to window object's foo property.

#### What is the outcome of the two alerts below?
```
var foo = "Hello";

(function() {
  var bar = " World";
  alert(foo + bar);
})();

alert(foo + bar);
```
It will alert "Hello World", then throws a reference error because there is no bar variable defined in the global scope.

#### What is the value of `foo.length`?
```
var foo = [];
foo.push(1);
foo.push(2);
```
foo.length is 2 as we call push() twice in the above code, so there are two items in the foo array.

#### What is the value of `foo.x`?
```
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
```
This one is really tricky. The answer is 'undefined'. Here is why:

In "foo.x = foo = {n: 2};", foo.x is first evaluated to 'undefined' since there is no x property in the object that foo refers. This is because left hand side of an assignment expression is evaluated first. So the whole expression is evaluated as it is "foo.x = (foo = {n: 2});"

#### What does the following code print?

```
console.log('one');
setTimeout(function() {
  console.log('two');
}, 0);
console.log('three');
```

The answer is:

one
three
two

This is because setTimeout() is asynchronous while console.log() is synchronous. setTimeout() schedules something to happen in the future while not blocking the normal flow of the program.
