## Node.js

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
