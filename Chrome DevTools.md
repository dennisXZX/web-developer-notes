## Chrome DevTools

### Tell Chrome debugger to ignore certain libraries

First go to the `three dots drop down menu` and select `Settings`, then select the `Blackboxing` tab where you can add scripts that you want the Chrome debugger to ignore. Hooray! Debugging becomes smoother after blackboxing.

### Console Logging

__Print DOM representation of an HTML element__

``` 
console.log(document.querySelector('.ei-activity-summary-page'));
```

__Warning message__

You can use console.warn() to specify warning messages, such as warning about deprecated APIs in future versions.

__String substitutions__

Console messages also support string substitutions.

- %s for string
- %d for number
- %o for object
- %c for CSS style

```
console.log('My name is %s. %cI am %d years old.', 'Dennis', 'color: blue; font-size: 30px', 35);
```

__Grouping log messages__

Console.group() or console.groupCollapsed() can be used to visually group together relevant messages.

```
for (let i = 0; i < 100; i++) {
  const num = Math.random() * 100;
  if (num > 50) {
    console.group('Number larger than 50');
    console.log(num);
    console.groupEnd();
  }
}
```

__Assertion__

Writes an error message to the console if the assertion is false. If the assertion is true, nothing happens.

This assertion can be used to quickly determine if an element is on the page.

```
// check if there is an element with class="container"
console.assert(document.querySelector('.container'), "Missing element with a container class element");
```
It can also be used to check on any expression.
```
let salary = '32';
// error message will show as salary is not type of number
console.assert(typeof salary === 'number', 'salary was not a number!');
```

__Tabularization__

You can log out an object in a tabularized manner.

```
console.table(object);
```

__Timer__

You can record the time of your code by using console.time() and console.timeEnd().

```
console.time('save user');

// code to save a user to database

console.timeEnd('save user');
```
