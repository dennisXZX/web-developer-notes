# Knockout.js

- [Observables](#observable)
- [Bindings](#bindings)
- [Components](#components)

## Observables

#### Observable array only tracks which objects are in the array, not their state

```
// notice the object in the observable array is just plain object, not observable. Therefore, any change to these objects will not be notified to the observable array. The observable array is notified when any object is added to it or gremoved from it.
this.products = ko.observableArray([
	{ model: 'Taylor', price: 134 },
	{ model: 'King', price: 234 },	
]);
```

#### The right-most properties don't require () in HTML declarative data binding

```
// people observable array is not the right-most property so it has to be called
<div data-bind="text: people().length"></div>
<div data-bind="text: address().city"></div>

// people observable array is the right-most so no parentheses are required
<div data-bind="foreach: people"></div>
```

#### Dynamically display the number of items
```
There are <span data-bind="text: myItems().length"></span> items
```

#### Enable or disable a button according to the length of an array
```
<button data-bind="enable: myItems().length < 5">Add</button>
```

#### Read and write an observable

Read an observale's value.
```
const myViewModel = function() {
    this.personName = ko.observable('Bob');
    this.personAge = ko.observable(123);
};

let name = myViewModel.personName();
```

Write an observable's value.
```
myViewModel.personName('Dennis');

// this method chain will write both personName and personAge observables
myViewModel.personName('Mary').personAge(50);
```

Create an observable array.
```
const myViewModel = function() {
  // itemToAdd is bound to an input in the view
  this.itemToAdd = ko.observable("");
	
  this.myObservableArray = ko.observableArray([
    { name: "Bungle", type: "Bear" },
    { name: "George", type: "Hippo" },
    { name: "Zippy", type: "Unknown" }
  ]);
	
  this.addItem = () => {
    this.myObservableArray.push(this.itemToAdd());
  };
};
```
The observable array has most of the built-in native array functions and a couple of more provided by Knockout.js.

You can use pure computed observables to utilize the performance gain provided by Knockout.js. One thing to note is that ko.pureComputed() can only get observable values but not set them.
```
this.fullName = ko.pureComputed(() => {
    return this.firstName() + " " + this.lastName();
});
```

#### Subscribe to an observable to monitor changes

The subscribe function accepts three parameters: `callback` is the function that is called whenever the notification happens, `target` (optional) defines the value of this in the callback function, and `event` (optional; default is "change") is the name of the event to receive notification for.

```
myViewModel.personName.subscribe((newValue) => {
    alert("The person's new name is " + newValue);
}, target, event);
```

## [Bindings](#bindings)

#### Bind the view model to a specific part of DOM

The `ko.applyBindings` is used to bind view to view model. The second parameter is used to define which part of the document you want to search for data-bind attributes. This is useful when you want to bind multiple view models to the different regions of a page.
```
ko.applyBindings(myViewModel, document.getElementById('someElementId'));
```

#### Binding context

There are some useful binding context.

- `$root` refers to the main view model object in the root context, usually the object that was passed to ko.applyBindings().

- If you’re within the context of a particular component template, then `$component` refers to the viewmodel for that component.

- `$parent` refers to the view model object in the immediate parent context. 

#### Express if/else conditional statement in Knockout

Console log inside a Knockout template.
```
<div data-bind="text: console.log(amount_option)"></div>
```

There is no if/else statement in Knockout, but we can achieve that with a trick.

```
<!-- ko ifnot: activityItem.shortDescription -->
	<p data-bind="text: activityItem.name"></p>
<!-- /ko -->
<!-- ko if: activityItem.shortDescription -->
	<p data-bind="text: activityItem.shortDescription"></p>
<!-- /ko -->
```

The difference between `visible` binding and `if` binding is that `visible` binding use CSS to toggle the element's visiblity, while `if` physically adds or removes the markup in the DOM.

#### value binding

There are a couple of `valueUpdate` properties that can be added to value binding to change how event is detected.

```
// most frequently used valueUpdate properties are 'keyup', 'keypress' and 'afterkeydown'
<input type="text" data-bind="value: price, valueUpdate: 'afterkeydown'" />
```

The difference between `textInput` and `value` binding is `textInput` provides immediate updates to the view model while `value` only updates your view model when user moves focus out of the text box. In addition, `textInput` provides better browser quirks handling.

#### css binding
The `css` binding adds or removes one or more named CSS classes to the associated DOM element.

#### style binding

The `style` binding adds or removes one or more style values to the associated DOM element.

#### attr binding

The `attr` binding provides a generic way to set the value of any attribute for the associated DOM element.

#### options binding

The `options` binding controls what options should appear in a drop-down list. If the array provided to the options binding is an array of arbitrary JavaScript objects, then `optionsText` and `optionsValue` parameters are needed.

- optionsCaption: the selected option by default
- optionsText: specify which of the objects' properties that should be displayed as the text in the drop-down list
- optionsValue: specify which of the objects’ properties should be used to set the value attribute
- optionsAfterRender: a callback function that will run after the option elements are generated

#### custom binding handler

You can create custom binding handler in Knockout.js.

```
ko.bindingHandlers.fadeVisible = {
	// runs the first time the binding is evaluated
	init: function(element, valueAccessor, allBindings, viewModel, bindingContext) {
	
	},
	// run every time one of its observables changes after init
	update: function(element, valueAccessor, allBindings, viewModel, bindingContext) {
	
	}
}
```

## Components

Register a component so the view model can be accessed in the template.
```
ko.components.register('ei-accommodation-search-page', {
	viewModel: AccommodationSearchPage,
	template: Template
});
```
