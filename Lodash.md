# Lodash

#### _.mapKeys

mapKeys can 

```
const posts = [
  { id: 4, title: 'ho' },
  { id: 5, title: 'yo' }
]

/* The specified property will be set as the key of the generated object
{
  "4": {
    { id: 4, title: 'ho' },
  },
  "5" {
    { id: 5, title: 'yo' }
  }
}
*/
_.mapKeys(posts, 'id');
```

#### _.sortedIndex, _.sortedIndexBy

sortedIndex can be used to insert an element to an array and maintain its order.

```
let collection = [
  'Carl',
  'Gary',
  'Luigi',
  'Otto'
];

const name = 'Luke';

// _.sortedIndex(collection, name) will return the index where the name should be in the collection array
// collection.splice(index, 0, name) will then insert name to the calculated index
collection.splice(_.sortedIndex(collection, name), 0, name);
```

sortedIndexBy can be used to insert an object element to an array and maintain its order.

```
let collection = [
  { name: 'Zoe' },
  { name: 'Tom' },
  { name: 'Ken' },
  { name: 'Dennis' }
];

const luke = {name: 'Luke'};

// order the collection array by name property
const newCollection = _.orderBy(collection, 'name');

// _.sortedIndexBy(newCollection, luke, 'name') returns the location where luke object should be in the newCollection array
newCollection.splice(_.sortedIndexBy(newCollection, luke, 'name'), 0, luke);
```

#### _.forEach & _.forEachRight

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
Example 1

// _.sortBy('cba') returns an array of [ 'a', 'b', 'c' ]
_.sortBy('cba').join(''); // => 'abc'

Example 2

const users = [
  { 'user': 'fred',   'age': 48 },
  { 'user': 'barney', 'age': 36 },
  { 'user': 'fred',   'age': 40 },
  { 'user': 'barney', 'age': 34 }
];

// sort the array by multiple property names in pluck style shorthand
_.sortBy(users, ['user', 'age']);

Example 3

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
_.times(5, () => {
  console.log('hi')
});
```
