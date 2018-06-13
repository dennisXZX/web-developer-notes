## Javascript Functional

#### Composition

```js
const people = require('./people.json');

const filterEq = (prop, value, arr) => {
    return arr.filter((elem) => {
        return elem[prop] === value;
    });
};
const filterNEq = (prop, value, arr) => {
    return arr.filter((elem) => {
        return elem[prop] !== value;
    });
};

// define a function that accepts multiple functions as parameters
const compose = (...funcs) => {
    // compose returns a function    
    return (initalData) => {
        // iterate through all the functions, executing each function with the data returned from the last function call
        // each time the value will become the return value from the function call
        return funcs.reduceRight((value, func) => {
            return func(value);
        }, initalData);
    };
};

const filterMarried = (arr) => filterEq('married', true, arr);
const filterSingle = (arr) => filterEq('married', false, arr);
const filterWomen = (arr) => filterEq('gender', 'Female', arr);
const filterMen = (arr) => filterNEq('gender', 'Female', arr);

// compose functions
const marriedWomen = compose(filterWomen, filterMarried);
const marriedMen = compose(filterMen, filterMarried);
const singleWomen = compose(filterWomen, filterSingle);
const singleMen = compose(filterMen, filterSingle);

const out = marriedWomen(people);
```

#### Currying

```js
// curry a function from accepting 3 parameters to 3 parameters
const isMarried = (value, arr) => filterEq('married', value, arr);

// curry a function from accepting 2 parameters to 1 parameter
const married = (arr) => isMarried(true, arr);
const single = (arr) => isMarried(false, arr);

const out = married(people);
```
