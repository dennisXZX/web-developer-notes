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