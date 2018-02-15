## RxJS

#### Observables, observers and subscriptions

```ts
Rx.Observable.fromEvent(button, 'click')
  .subscribe(
    (value) => console.log(value.clientX), // next function
    (error) => {...}, // error function
    () => {...} // complete function
  );
  
// you can also pass an observer object to the subscription
// define an observer function
const observer = {
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
	.subscribe(observer);
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
      this.logger.log(`Got ${custs.length} customers`);
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
    .do(custs => this.logger.log(`Got ${custs.length} customers`))
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
