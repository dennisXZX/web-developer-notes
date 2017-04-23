These are the things you should know as a web developer.

## Table of Contents

  1. [HTML](#html)
  1. [CSS](#css)
  1. [Javascript](#javascript)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Network](#network)
  1. [Database](#database)
  1. [Coding](#coding)

#### HTML

* What does a `doctype` do?
* What's the difference between full standards mode, almost standards mode and quirks mode?
* What's the difference between HTML and XHTML?
* Are there any problems with serving pages as `application/xhtml+xml`?
* How do you serve a page with content in multiple languages?
* What kind of things must you be wary of when design or developing for multilingual sites?
* What are `data-` attributes good for?
* Consider HTML5 as an open web platform. What are the building blocks of HTML5?
* Describe the difference between a `cookie`, `sessionStorage` and `localStorage`.
* Describe the difference between `<script>`, `<script async>` and `<script defer>`.
* Why is it generally a good idea to position CSS `<link>`s between `<head></head>` and JS `<script>`s just before `</body>`? Do you know any exceptions?
* What is progressive rendering?
* Have you used different HTML templating languages before?
* Explain what ARIA and screenreaders are, and how to make a website accessible.

#### CSS

* What is the difference between classes and IDs in CSS?
```
ID is unique while you can have multiple classes with the same name in a document.
```
* What's the difference between "resetting" and "normalizing" CSS? Which would you choose, and why?
* Describe Floats and how they work.
* Describe z-index and how stacking context is formed.
* Describe BFC(Block Formatting Context) and how it works.
* What are the various clearing techniques and which is appropriate for what context?
* Explain CSS sprites, and how you would implement them on a page or site.
* What are your favourite image replacement techniques and which do you use when?
* How would you approach fixing browser-specific styling issues?
* How do you serve your pages for feature-constrained browsers?
  * What techniques/processes do you use?
* What are the different ways to visually hide content (and make it available only for screen readers)?
* Have you ever used a grid system, and if so, what do you prefer?
* Have you used or implemented media queries or mobile specific layouts/CSS?
* Are you familiar with styling SVG?
* How do you optimize your webpages for print?
* What are some of the "gotchas" for writing efficient CSS?
* What are the advantages/disadvantages of using CSS preprocessors?
  * Describe what you like and dislike about the CSS preprocessors you have used.
* How would you implement a web design comp that uses non-standard fonts?
* Explain how a browser determines what elements match a CSS selector.
* Describe pseudo-elements and discuss what they are used for.
* Explain your understanding of the box model and how you would tell the browser in CSS to render your layout in different box models.
* What does ```* { box-sizing: border-box; }``` do? What are its advantages?
* List as many values for the display property that you can remember.
* What's the difference between inline and inline-block?
* What's the difference between a relative, fixed, absolute and statically positioned element?
* The 'C' in CSS stands for Cascading.  How is priority determined in assigning styles (a few examples)?  How can you use this system to your advantage?
* What existing CSS frameworks have you used locally, or in production? How would you change/improve them?
* Have you played around with the new CSS Flexbox or Grid specs?
* How is responsive design different from adaptive design?
* Have you ever worked with retina graphics? If so, when and what techniques did you use?
* Is there any reason you'd want to use `translate()` instead of *absolute positioning*, or vice-versa? And why?
* Explain some of the pros and cons for CSS animations versus JavaScript animations.

#### Javascript

* In JavaScript, what's the difference between this, $(this) and $this?
```
‘this’ is a Javascript keyword. The value of ‘this’ varies depending on how a function is invoked. Mainly there are four different patterns.

- when invoked as a function, ‘this’ refers to the window object. 
- when invoked as a method, ‘this’ refers to the object that calls the method.
- when invoked as a constructor function, ‘this’ refers to the object instance created by the constructor.
- when invoked by using call() and apply() method, ‘this’ refers to the object passed in as the first parameter.

In ES6, an arrow function does not create its own 'this' context, so 'this' has its original meaning from the enclosing context.

$(this) is not a legitimate Javascript variable. In jQuery library, however, it means to construct a jQuery object so can call jQuery methods on the object.

$this is a legitimate Javascript variable.
```
* Explain event delegation
* Explain how `this` works in JavaScript
* Explain how prototypal inheritance works
* What do you think of AMD vs CommonJS?
* Explain why the following doesn't work as an IIFE: `function foo(){ }();`.
  * What needs to be changed to properly make it an IIFE?
* What's the difference between a variable that is: `null`, `undefined` or undeclared?
  * How would you go about checking for any of these states?
* What is a closure, and how/why would you use one?
* What's a typical use case for anonymous functions?
* How do you organize your code? (module pattern, classical inheritance?)
* What's the difference between host objects and native objects?
* Difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?
* What's the difference between `.call` and `.apply`?
* Explain `Function.prototype.bind`.
* When would you use `document.write()`?
* What's the difference between feature detection, feature inference, and using the UA string?
* Explain Ajax in as much detail as possible.
* What are the advantages and disadvantages of using Ajax?
* Explain Cross-Origin Resource Sharing (CORS)
```
CORS is a mechanism that allows you to work around the same-orign policy implemented by browsers. By enabling CORS on your server, you sepcify what other servers can have access to your resources. Therefore, your server would respond to requests with a Access-Control-Allow-Origin header to let the browser know if the requested resource is accessible to those origins.
```
* Explain how JSONP works
```
JSONP stands for JSON with Padding, yet another poorly named term in the programming field. It is a technique to address the same domain policy implemented in the browser land, which relies on <script> tags to bypass the restriction.

Say inn a JSONP enabled server, you send a request http://www.example.net/sample.aspx?callback=mycallback to it, it then will return a result wrapped in the callback function you specified.

mycallback({ foo: "happy coder" });

So, in your program, you can define the callback function to handle the response:

mycallback = (data) => {
  alert(data.foo);
};
```
* Have you ever used JavaScript templating?
  * If so, what libraries have you used?
* Explain "hoisting".
* Describe event bubbling.
* What's the difference between an "attribute" and a "property"?
* Why is extending built-in JavaScript objects not a good idea?
* Difference between document load event and document DOMContentLoaded event?
* What is the difference between `==` and `===`?
* Explain the same-origin policy with regards to JavaScript.
* Make this work:
```javascript
duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
```
* Why is it called a Ternary expression, what does the word "Ternary" indicate?
* What is `"use strict";`? what are the advantages and disadvantages to using it?
* Create a for loop that iterates up to `100` while outputting **"fizz"** at multiples of `3`, **"buzz"** at multiples of `5` and **"fizzbuzz"** at multiples of `3` and `5`
* Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?
* Why would you use something like the `load` event? Does this event have disadvantages? Do you know any alternatives, and why would you use those?
* Explain what a single page app is and how to make one SEO-friendly.
* What is the extent of your experience with Promises and/or their polyfills?
* What are the pros and cons of using Promises instead of callbacks?
* What are some of the advantages/disadvantages of writing JavaScript code in a language that compiles to JavaScript?
* What tools and techniques do you use debugging JavaScript code?
* What language constructions do you use for iterating over object properties and array items?
* Explain the difference between mutable and immutable objects.
  * What is an example of an immutable object in JavaScript?
  * What are the pros and cons of immutability?
  * How can you achieve immutability in your own code?
* Explain the difference between synchronous and asynchronous functions.
* What is event loop?
  * What is the difference between call stack and task queue?
* Explain the differences on the usage of `foo` between `function foo() {}` and `var foo = function() {}`

#### Testing

* What are some advantages/disadvantages to testing your code?
* What tools would you use to test your code's functionality?
* What is the difference between a unit test and a functional/integration test?
* What is the purpose of a code style linting tool?

#### Performance

* What tools would you use to find a performance bug in your code?
* What are some ways you may improve your website's scrolling performance?
* Explain the difference between layout, painting and compositing.

#### Network

* Traditionally, why has it been better to serve site assets from multiple domains?
* Do your best to describe the process from the time you type in a website's URL to it finishing loading on your screen.
* What are the differences between Long-Polling, Websockets and Server-Sent Events?
* Explain the following request and response headers:
  * Diff. between Expires, Date, Age and If-Modified-...
  * Do Not Track
  * Cache-Control
  * Transfer-Encoding
  * ETag
  * X-Frame-Options
* What are HTTP methods? List all HTTP methods that you know, and explain them.

#### Database

##### Relational

##### NOSQL

#### Coding

*Question: What is the value of `foo`?*
```javascript
var foo = 10 + '20';
```
```
foo is a string with a value of '1020'. 

This is because when you try to concatenate a number with a string, the number will be automatically converted into a string before concatenation.
```

*Question: How would you make this work?*
```javascript
add(2, 5); // 7
add(2)(5); // 7
```
```
function add(num1, num2) {
	if(arguments.length === 1) {
		return function(num2) {
			return num1 + num2;
		}
	}
	return num1 + num2;
}
```

*Question: What value is returned from the following statement?*
```javascript
"i'm a lasagna hog".split("").reverse().join("");
```
```
After the method chain, the returned value is 'goh angasal a m'i'. 

First the string is split into an array of characters because the split() function is called passing an empty string as parameter. After that, the array is reversed then joined together.
```

*Question: What is the value of `window.foo`?*
```javascript
( window.foo || ( window.foo = "bar" ) );
```
```
The value of window.foo is 'bar'.

This expression first evaluates the left hand side of the || operator, which is a property retrival expression that produces an undefined value. Then it evaluates the right hand side, which is an assignment expression that assigns a string 'bar' to window object's foo property.
```

*Question: What is the outcome of the two alerts below?*
```javascript
var foo = "Hello";
(function() {
  var bar = " World";
  alert(foo + bar);
})();
alert(foo + bar);
```
```
It will alert "Hello World", then throws a reference error because there is no bar varialbe defined in the global scope.
```

*Question: What is the value of `foo.length`?*
```javascript
var foo = [];
foo.push(1);
foo.push(2);
```
```
foo.length is 2 as we call push() twice in the above code, so there are two items in the foo array.
```

*Question: What is the value of `foo.x`?*
```javascript
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
```
```
This one is really tricky. The answer is 'undefined'. Here is why:

In "foo.x = foo = {n: 2};", foo.x is first evaluated to 'undefined' since there is no x property in the object that foo refers. This is because left hand side of an assignment expression is evaluated first. So the whole expression is evaluated as it is "foo.x = (foo = {n: 2});"
```

*Question: What does the following code print?*
```javascript
console.log('one');
setTimeout(function() {
  console.log('two');
}, 0);
console.log('three');
```
