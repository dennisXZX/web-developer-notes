## DOM

#### Retrieve an element's property using Javascript

```js
// select an element
const para = document.querySelector('p');

// get all its current style, whether is from inline, style tag or external
const compStyles = window.getComputedStyle(para);

// get the width value
const paraWidth = compStyles.getPropertyValue('width');
```

#### Convert HTMLCollection or NodeList to normal array

```js
let scriptsArr = Array.from(document.scripts);

scriptsArr.forEach(script => {
  console.log(script.getAttribute('src'))
})
```

#### Mouseenter vs Mouseover

The mouseenter event is only triggered when the mouse pointer enters the element associated with the event.

The mouseover event triggers when the mouse pointer enters the element associated with the event and its child elements.

#### Cool DOM trick

Change background color using mouseover event

```js
document.addEventListener('mousemove', (e) => {
  document.body.style.backgroundColor = `rgb($(e.offsetX), ${e.offsetY}, 40)`
})
```

#### Event delegation

Event delegation is used when you want to add an event listener to an element, which is added to the DOM dynamically after the event listener registration. In such a case, we normally attach an event listener to the parent element and monitor whether events happen to some particular children.

```js
// add an event listener to the parent
document.body.addEventListener('click', deleteItem)

function deleteItem (e) {
  // monitor if some particular children elements are being clicked
  if (e.target.classList.contains('delete-item')) {
    e.target.parentElement.remove()
  }
}
``` 
