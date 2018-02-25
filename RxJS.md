## RxJS

#### reduce and scan

Both reduce() and scan() can be used to reduce a bunch of values into a single one. The difference is that scan() would also emit the value in each step of the reduce process.

#### debounceTime

```ts
Rx.Observable.fromEvent(input, 'input') // convert  the input event into an observable
  .map(event => event.target.value) // extract the value from the input field
  .debounceTime(500) // emit the value only there is a pause
  .distinctUntilChanged() // emit the value only the current value is different with the last one
  .subscribe({
    next: (value) => console.log(value)
  })
```

#### Subject

Subjects are observables themselves but what sets them apart is that they are also observers. In addition, subject supports multiple subscriptions so you can register multiple observers on a subject.

```ts
const subject = new Rx.Subject();

const observer1 = {
  next: (value) => console.log(value + '1')
}

const observer2 = {
  next: (value) => console.log(value + '2')
}

// you can subscribe to multiple observers
subject.subscribe(observer1);
subject.subscribe(observer2);

// you can emit value from a subject
subject.next(10);
```

#### Operators

We chain RxJS operators to an observable before subscribing it.

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
  
// you can also define observer in a 'formal' way
class MyObserver implements Observer<number> {
  next: (value) => {
    console.log(value);
  },
  error: (error) => {
    console.log(error);
  },
  complete: () => {
    console.log('completed');
  }
}

Rx.Observable.fromEvent(button, 'click')
  .subscribe(new MyObserver());
```

#### Convert to observables

```ts
import { Observable } from 'rxjs';

// convert observable from an array
Observable.from([1, 5, 10]);

// convert observable from an event
Rx.Observable.fromEvent(button, 'click');
```

#### Emit an event when specified duration has passed

```ts
const button = document.querySelector('button');

Rx.Observable.fromEvent(button, 'click') // create observable that emits click events
  .throttleTime(1000) // emit the latest value when 1000 milliseconds has passed
  .map((event) => { return event.clentY }) // map the click event and return its vertical coordinate
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
    alert(errorMsg); // Don't use alert!
  });

this.dataService.getCustomers().subscribe( // Observable version
    (custs) => {
      this.isBusy = false;
      this.customers = custs;
    },
    (errorMsg: string) => {
      this.isBusy = false;
      alert(errorMsg); // Don't use alert!
    }
  );
```
