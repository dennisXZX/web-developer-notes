## Babel

#### Set up Babel

Install necessary NPM packages for a React project.

```js
npm i -D babel-loader babel-core babel-preset-react babel-preset-env
```

Create a Babel config file `.babelrc`. You can find the browser list setting for babel-preset-env [here](https://github.com/ai/browserslist).

```js
{
  "presets": [
    ["env", {
      "targets": {
        "browsers": [
          "> 1%",
          "last 2 versions"
        ]
      },
     // use polyfill for features like Promises and Object.assign() in older browsers
     useBuiltIns: 'entry'
    }],
    "stage-2",
    "react"
  ],
  "plugins": [
    // support dynamic import in your code
    "syntax-dynamic-import"
  ]
}
```
