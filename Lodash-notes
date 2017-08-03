# Lodash Notes

#### Install/require only the used modules from Lodash

// only install the foreach method from the lodash library
npm install --save lodash.foreach
```
// require the foreach method from the module
var forEach = require('lodash.foreach');
// require only the foreach module form lodash
var forEach = require('lodash/foreach');
```

#### Stop iterating in a forEach method once a result is found

```
var collection = [
    'Timothy',
    'Kelly',
    'Julia',
    'Leon'
];

_.forEach(collection, function(name, index) {
    if (name === 'Kelly') {
        console.log('Kelly Index: ' + index);
        return false; // use return false to quit a forEach iteration
    }
});
```

#### _times

```
_.times(5, function(){
    console.log('hi')
});
```
