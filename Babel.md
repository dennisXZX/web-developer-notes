## Babel

#### Plugin vs Preset

In Babel world, each plugin transpiles one feature, for example, a plugin will be responsible for transpiling arrow function into normal function. It would be a headache if we need to manually install all the plugins, so that's why preset comes to rescue. Preset is nothing but a group of plugins.

#### babel-polyfill

Babel transpiles our code to something that browsers can understand, but the resulting code might use features that may or may not work in every single browser. For example Object.assign is not supported in all browsers, so `babel-polyfill` fills the holes. It's just a collection of polyfills that you would usually include anyway to have support for legacy browsers.

To use `babel-polyfill`, you can import it in the top level component.

```js
import 'babel-polyfill';
```

#### Set up Babel

Install necessary NPM packages for a React project.

```
npm i -D babel-loader babel-core babel-preset-react babel-preset-env babel-plugin-syntax-dynamic-import babel-polyfill
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
     useBuiltIns: 'entry',
     // not worry about the edge case, which would make the bundle size smaller
     "loose": true,
     // skip any module code (import React from 'react'), leave Webpack to worry about it
     "modules": false
    }],
    // using stage-2 features described in The TC39 Process
    "stage-2",
    "react"
  ],
  "plugins": [
    // support dynamic import in your code
    "syntax-dynamic-import"
  ]
}
```
