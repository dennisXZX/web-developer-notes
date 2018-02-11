## RxJS

#### Promise vs Observable

```js
/** Get existing customers as a Promise */
getCustomersPromise(): Promise<Customer[]> {
  this.logger.log('Getting customers as a Promise ...');

  // retrieve the customers
  const customers = createTestCustomers();

  return new Promise(resolve => {
    setTimeout(() => {
      this.logger.log(`Got ${customers.length} customers`);
      resolve(customers);
    }, 1500); // simulate server response latency
  });
}

/** Get existing customers as an Observable */
getCustomersObservable(): Observable<Customer[]> {
  this.logger.log('Getting customers as an Observable ...');

  // retrieve the customers
  const customers = createTestCustomers();

  return of(customers) // convert to an observable
    .delay(1500) // simulate server response latency
    .do(custs => this.logger.log(`Got ${custs.length} customers`));
}
```

Here is the difference between consuming a promise and an observable.

```js
this.dataService.getCustomersPromise().then( // promise version
  custs => {
    this.isBusy = false;
    this.customers = custs;
  });

this.dataService.getCustomersObservable().subscribe( // observable version
  custs => {
    this.isBusy = false;
    this.customers = custs;
  });
```
