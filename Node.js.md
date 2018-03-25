## Node.js

#### Fundamentals

Node.js is not a programming language or framework. It is a runtime environment written in C++ for executing Javascript code with the V8 Javascript engine embedded. It is suitable for real time application but not CPU-intensive application.

When we declare a variable in a client-side Javascript environment (browser), it is automatically added to the `window` global object. However, in Node environment, the variable is scoped to the file in which it is declared and would not contaminate the `global` object. This is because in Node, every file is a separate module, which means variables and functions declared inside can only be accessed within the file. Node achieves this modularity by wrapping your code in an IIFE `function (exports, require, module, __filename, __dirname)` in each file. If you want to expose any variable or function to other module. You need to explicitly export them in the module. 

```js
const url = 'http://dennisxiao.com';

function log(message) {
  // send an http request
  console.log(message);
}

// export a variable called endPoint
module.exports.endPoint = url;
module.exports.log = log;

// check the info of the current module
console.log(module);
```

You can export just a function by using `module.exports = log`.

In order to load a module, you need to use `require()`.

```js
const logger = require(./logger);
```

It is noted that in a Node module, the keyword `this` refers to `module.exports`.

#### Set up a NODE_PATH

When you require modules in a complex project, it would get messy soon when you have to deal with file paths. You can set up a `NODE_PATH` in your package.json file to specify your module path.

```js
"scripts": {
  // set NODE_PATH to the lib folder, so all the files use this 
  "start": "NODE_PATH=./lib node index.js"
}
```

#### Node REPL

Get into Node REPL shell (Read–Eval–Print Loop) by typing `node`.

In Node REPL, the `this` keyword is equal to the `global` object, however, in any Node module, the top level `this` refers to `module.exports`.

In Node REPL, the `_` is set to be the result of the last operation.

```
1 + 1 ==> 2

_ + 1 ==> 3
```

- `.save fileName.js` and `.load fileName.js` to save / load a file
- `.editor` to get into editor mode
- `.help` to get help
- `.exit` to exit Node REPL
