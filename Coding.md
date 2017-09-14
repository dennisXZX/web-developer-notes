## Coding

#### Get the mean value from an array

```
/*
Requirements:
Double the value of each element in the array and then get its average value.
*/
const data = [1, 2, 3, 3, 4, 5, 3, 1];
```
Solution:

```
const mean = data.reduce((accumulator, currentItem, index, array) => {
  let intermediaryValue = accumulator + (currentItem * 2);

  if (index === array.length - 1) {
    return intermediaryValue / array.length;
  }

  return intermediaryValue;

}, 0);
```

#### Array spliting

```
// data array
const arr = ['a', 'b', 'c', 'd', 'e', 'f'];

// expected results
splitArr(arr, 2); // => ["ab", "cd", "ef"]
splitArr(arr, 3); // => ["abc", "def"]
```
Solution:

```
function splitArr(arr, splitNum) {
  // join the array
  joinArr = arr.join('');
  // create a regular expression pattern
  // "." matches any single character except the newline character.
  // "{n,m}" matches at least n and at most m occurrences of the preceding expression.
  const pattern = ".{1," + splitNum + "}";
  // "g" flag for global search.
  const reg = new RegExp(pattern, "g");
  // match() returns an array of information or null on a mismatch.
  processedArr = joinArr.match(reg);
  return processedArr;
}
```

#### Convert an array to an object

```
// original format
let collection = [
  { id: 1, age_from: 12, age_to: 99, name: "Adult" },
  { id: 2, age_from: 3, age_to: 16,  name: "Child" },
  { id: 3, age_from: 0, age_to: 2,  name: "Infant" }
];

// target format
let newArr = {
  '1': { name: 'Adult', age_from: 12, age_to: 99 },
  '2': { name: 'Child', age_from: 3, age_to: 16 },
  '3': { name: 'Infant', age_from: 0, age_to: 2 }
}
```

Solution:

```
const newCollection = collection.reduce((acc, curr) => {
  acc[curr['id']] = {
    name: curr['name'],
    age_from: curr['age_from'],
    age_to: curr['age_to']
  }
  return acc;
}, {});

console.log(newCollection);
```

#### Convert arr to newArr

```
// original format
let arr = [1, 1, 1, 2, 2, 3, 3];

// target format
let newArr = [
  {"1": 3},
  {"2": 2},
  {"3": 2}
]
```

Solution:

```
// reduce to an obj { '1': 3, '2': 2, '3': 2 }
let obj = arr.reduce((acc, cur) => {
  if (!acc[cur]) {
    acc[cur] = 1;
  } else {
    acc[cur] += 1;
  }
  return acc;
}, {});

let newArr = [];

// loop through obj and create a new object for each key/value pair and push it to newArr
for (let key in obj) {
  let newObj = {};
  newObj[key] = obj[key];
  newArr.push(newObj);
}
```

#### What is the output of this for loop?
```
let i, 
    myarray = [1, 2, 3];

for(i = myarray.length; i--;) {
  console.log(myarray[i])
}
```
The output is 3, 2, 1. This for loop may seems weird at first glance. Let's dig a bit deeper into the structure of a for loop. A for loop consists of 4 parts, where initialExpression, condition and incrementExpression are optional.

for ([initialExpression]; [condition]; [incrementExpression])
  statement

So the above for loop omits the incrementExpression and uses condition to achieve the decrement. The variable i is 3 initially but when it enters the for loop the first time it becomes 2. So the first output value is actually myarray[2], which is 3.

It is not recommended to use this for loop structure to achieve micro performance optimization as it is not easily recognized, unless in performance-critical operations.

#### What is the value of `foo`?
```
var foo = 10 + '20';
```
foo is a string with a value of '1020'. 

This is because when you try to concatenate a number with a string, the number will be automatically converted into a string before concatenation.

#### How would you make this work?

```
add(2, 5); // 7
add(2)(5); // 7
```
Solution 1
```
function add(num1, num2) {
  if (arguments.length === 1) {
    return function(newNum2) {
      return num1 + newNum2;
    };
  }
  return num1 + num2;
}
```
Solution 2
```
function add(num1, num2) {
  if (typeof num2 === 'undefined') {
    return function (newNum2) {
      return x + newNum2;
    };
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

#### Write a function called double in a declarative way which takes in an array of numbers and returns a new array after doubling every item in that array. double([1,2,3]) -> [2,4,6]

```
function double(array) {
  return array.map(item => item * 2);
}
```

#### Write a function called add in a declarative way which takes in an array and returns the result of adding up every item in the array. add([1,2,3]) -> 6

```
function add(array) {
  return array.reduce((prev, next) => prev + next, 0);
}
```
