## RxJS

#### Import operators

```
// import core observable functionality
import { Observable } from 'rxjs/Observable';

// import operators
import 'rxjs/add/operator/map'
```

#### switchMap

```ts
const obs1 = Rx.Observable.fromEvent(button, 'click');
const obs2 = Rx.Observable.interval(1000);

// after the click event is triggered, it switches to the interval observable
// every new click event would cancel the old click event observable
obs1.switchMap(
  event => {
    return obs2;
  }
).subscribe(
  value => console.log(value)
);
```

#### mergeMap

```ts
const obs1 = Rx.Observable.fromEvent(input1, 'input');
const obs2 = Rx.Observable.fromEvent(input2, 'input');

// mergeMap will emit a new value only the inner observable changes
// so here only when the input value of the input2 changes, the combinedValue would change
obs1.mergeMap(
  event1 => {
    return obs2.map(
      event2 => event1.target.value + ' ' + event2.target.value
    )
  }
).subscribe(
  combinedValue => span.textContent = combinedValue
);
```

#### reduce and scan

Both reduce() and scan() can be used to reduce a bunch of values into a single one. The difference is that scan() would also emit the value in each step of the reduce process.

#### debounceTime

```ts
Rx.Observable.fromEvent(input, 'input') // convert the input event into an observable
  .map(event => event.target.value) // extract the value from the input field
  .debounceTime(500) // discard emitted values that take less than half a second between output
  .distinctUntilChanged() // emit a value only the current value is different with the last one
  .subscribe({
    next: (value) => console.log(value)
  })
```

#### Subject, BehaviorSubject, ReplaySubject

Subjects are observables themselves but what sets them apart is that they are also observers. In addition, subject supports multiple subscriptions so you can register multiple observers on a subject.

```ts
const subject = new Rx.Subject();

const observer1 = {
  next: (value) => console.log(value + 'A')
}

const observer2 = {
  next: (value) => console.log(value + 'B')
}

// you can subscribe to multiple observers
subject.subscribe(observer1);
subject.subscribe(observer2);

// you can emit value from a subject
subject.next(10);
```

A BehaviorSubject holds one value and can be created with an initial value `new Rx.BehaviorSubject(1)`. When it is subscribed it emits the value immediately. A Subject doesn't hold a value and is triggered only on `.next(value)` call. Consider ReplaySubject if you want the subject to hold more than one value.

#### Operators

We chain RxJS operators to an observable before subscribing to it.

```ts
// create an observable which emits a sequential number every 1 second
const observable = Rx.Observable.interval(1000);

// create an observer, since the observable created from interval() would never end and throw an error
// we do not need to define error and complete functions
const observer = {
  next: (value) => {
    console.log(value);
  }
}

function isEven(value) {
  if (value % 2 === 0) {
    return value + ' is even';
  } else {
    return value + ' is odd';
  }
}

// chain operators to the observable
// and subscribe to it at the end
observable
  .map((value) => isEven(value))
  .throttleTime(2000)
  .subscribe(observer);
```

#### Observables, observers and subscriptions

```ts
// an easy way to define an observer is to pass three functions directly to the subscription
// the error function and complete function are optional
Rx.Observable.fromEvent(button, 'click')
  .subscribe(
    (value) => console.log(value.clientX), // next function
    (error) => {...}, // error function
    () => {...} // complete function
  );
  
// you can also define an observer in a 'formal' way
class MyObserver implements Observer<number> {
  next(value) {
    console.log(value);
  },
  error(error) {
    console.log(error);
  },
  complete() {
    console.log('completed');
  }
}

Rx.Observable.fromEvent(button, 'click')
  .subscribe(new MyObserver()); // pass in an instance of the MyObserver
```

#### Convert to observables

```ts
const arr = [1, 2, 5];

// you can use a low-level API to create an observable from scratch
// the Observable.create() method accepts a function, which takes in an observer object that has methods like next(), error() and complete()
const observable = Rx.Observable.create((observer) => {

  // iterate the array and emit each value
  for (let i of arr) {
    if (i === 5) {
      observer.error('an error is thrown'); // throw an error
    }
    
    observer.next(i); // emit a value
  }
  
  observer.complete(); // call the complete method
});

// subscribe to the observable, passing in three functions as an observer
observable.subscribe(
  (value) => console.log(value),
  (error) => console.log(error),
  () => console.log('complete')
);
```

```ts
// convert observable from an array
Rx.Observable.from([1, 5, 10]);

// convert observable from an event
Rx.Observable.fromEvent(button, 'click');
```

#### Emit an event when specified duration has passed

```ts
const button = document.querySelector('button');

Rx.Observable.fromEvent(button, 'click') // create observable that emits click events
  .throttleTime(1000) // emit the latest value when 1000 milliseconds has passed
  .map(event => event.clentY) // map the click event and return its vertical coordinate
  .subscribe(
    (coordinate) => console.log(coordinate)
  )
```

#### Promise vs Observable

```js
/** Get existing customers as a Promise */
getCustomersPromise(): Promise<Customer[]> {
  this.logger.log('Getting customers as a Promise via Http ...');

  return this.http.get(this.customersUrl) // <-- returns an observable
    .toPromise() // <-- convert immediately to a promise
    .then(response => {
      const custs = response.json().data as Customer[]; // <-- extract data from the response
      this.logger.log(`Got ${custs.length} customers`); // use the logger service
      return custs;
    })
    .catch((error: any) => {
      this.logger.log(`An error occurred ${error}`); // for demo purposes only
      // re-throw user-facing message
      return Promise.reject('Something bad happened with customers; please check the console');
    });
}

/** Get existing customers as an Observable */
getCustomersObservable(): Observable<Customer[]> {
  this.logger.log('Getting customers as an Observable via Http ...');

  return this.http.get(this.customersUrl)
    .map(response => response.json().data as Customer[])  // <-- extract data
    .do(custs => this.logger.log(`Got ${custs.length} customers`)) // produce some side-effect
    .catch(error => this.handleError(error));
}
```

Here is the difference between consuming a promise and an observable.

```js
this.dataService.getCustomersPromise() // promise version
  .then(
    custs => {
      this.isBusy = false;
      this.customers = custs;
    })
  .catch(error => {
    this.isBusy = false;
    console.log(errorMsg);
  });

this.dataService.getCustomers().subscribe( // Observable version
    (custs) => {
      this.isBusy = false;
      this.customers = custs;
    },
    (errorMsg: string) => {
      this.isBusy = false;
      console.log(errorMsg);
    }
  );
```
