## Node.js

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
