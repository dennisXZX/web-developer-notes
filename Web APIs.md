## Web APIs

#### Detect screen size using window.matchMedia

```js
if (window.matchMedia("(min-width: 400px)").matches) {
  /* the viewport is at least 400 pixels wide */
} else {
  /* the viewport is less than 400 pixels wide */
}

if (window.matchMedia("(orientation: portrait)").matches) {
  /* The viewport is currently in portrait orientation */
} else {
  /* The viewport is not currently in portrait orientation, therefore landscape */
}
```

#### URLSearchParams

The URLSearchParams interface defines utility methods to work with the query string of a URL.

```js
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

#### localStorage and sessionStorage

```js
// set a local storage item
localStorage.setItem('name', 'Dennis')
localStorage.setItem('age', '36')

// remove a value from local storage
localStorage.removeItem('name')

// get a value from local storage
const name = localStorage.getItem('name')

// clear local storage
localStorage.clear()

// set session storage item, session storage clears out when you end the session (close the tab or browser)
sessionStorage.setItem('name', 'Dennis')
```

You would need to use `JSON.parse()` to read the value from the local storage, and `JSON.stringify()` to convert array or object into string before storing in local storage.
