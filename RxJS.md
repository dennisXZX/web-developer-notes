## RxJS

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
this.dataService.getCustomersPromise()
  .then( // promise version
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
