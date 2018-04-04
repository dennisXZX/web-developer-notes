## Yargs

Yargs helps you build interactive command line tools by parsing arguments and generating an elegant user interface.

```js
// store 2 default properties into argv
var argv = require('yargs')
    .default('buildnum', buildnum)
    .default('configPath', '/var/excite/e2t-static/config/')
    .argv;
```

so later in the code you can access these properties by the following:

```js
// import yargs argv object
var argv = require('yargs').argv;

// access properties from argv object
concat('build.'  + argv.buildnum + '.min.js', {separator: '\n\r;'}
```
