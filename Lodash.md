# Lodash Notes

#### _.forEach / _.forEachRight

```
var collection = [
  'Timothy',
  'Kelly',
  'Julia',
  'Leon'
];

_.forEach(collection, (name, index) => {
  if (name === 'Kelly') {
    console.log('Kelly Index: ' + index);
    return false; // use return false to quit a forEach iteration
  }
});
```

#### _.sortBy & _.orderBy

sortBy() works on all array-like collections and always returns an array.

```
// Example 1:
// _.sortBy('cba') returns an array of [ 'a', 'b', 'c' ]
_.sortBy('cba').join(''); // => 'abc'

// Example 2:
const users = [
  { 'user': 'fred',   'age': 48 },
  { 'user': 'barney', 'age': 36 },
  { 'user': 'fred',   'age': 40 },
  { 'user': 'barney', 'age': 34 }
];

// sort the array first by user property, then by age
_.sortBy(users, ['user', 'age']);

// sort summaryList array based on the result of the callback function
summaryList = sortBy(summaryList, (ageGroup) => {
  // returns the index where the ageGroupId appears in the order array
  return indexOf(order, ageGroup.ageGroupId);
});
```

orderBy() works exactly as sortBy(), excepts it offers the feature of specifiying the sort order.

// order the array first by user property in ascending order, then by age in descending order
_.orderBy(users, ['user', 'age'], ['asc', 'desc']);

#### _.times

times() can be used to execute a function a certain amount of times.

```
_.times(5, function(){
  console.log('hi')
});
```
