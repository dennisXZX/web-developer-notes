# Webpack Notes

#### Install webpack and run webpack without a config file

Install webpack
```
// the long command
npm install webpack --save-dev
// the short command
npm i webpack -D
```

Set up a script to run webpack in package.json (created by running `npm init`)
```
"script": {
  "build": "webpack src/js/entry.js dist/bundle.js",
  "build:prod": "webpack src/js/entry.js dist/bundle.js -p"
}
```
