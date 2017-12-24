## Web APIs

#### URLSearchParams

The URLSearchParams interface defines utility methods to work with the query string of a URL.

```
const params = new URLSearchParams('q=search+string&version=1&person=Eric');

// retrieve values from the query string
params.get('q') // "search string"
params.get('version') // "1"

/* loop through the query string
   ["q", "search string"]
   ["version", "1"]
   ["person", "Eric"]
*/

for (let p of params) {
  console.log(p);
}
```
