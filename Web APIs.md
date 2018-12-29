## Web APIs

#### Reduce the DOM operation by using createDocumentFragment

```js
const element  = document.getElementById('ul');
const fragment = document.createDocumentFragment();
const browsers = ['Firefox', 'Chrome', 'Opera',
  'Safari', 'Internet Explorer'];

browsers.forEach(function(browser) {
  const li = document.createElement('li');
  li.textContent = browser;
  // manipulate the document fragment instead of DOM directly
  fragment.appendChild(li);
});

// operate on the DOM only once
element.appendChild(fragment);
```

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

Since `cookie` has a size limit of 4KB and has to be sent with each AJAX request, local storage has now largely replaced cookie as the browser storage solution.

Session storage clears out when you end the session (close the tab or browser), while local storage will remain in your browser until you manually clear them out.

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
```

You would need to use `JSON.parse()` to read the value from the local storage, and `JSON.stringify()` to convert array or object into string before storing in local storage.
