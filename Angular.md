## Angular

#### How Angular app is bootstrapped

In `.angular-cli.json`, the 'main' property specifies where to look for the bootstrap file, which is `main.ts` by default. In main.ts, an Angular module is specified to bootstrap the app, which is `AppModule` by default. In `app.module.ts`, we specify which component to use as the top-level component in 'bootstrap' property, which is `AppComponent` by default. Also in this file, we specify what other components belong to this module in 'declarations' property.

#### Passing values from container to child component

Use bracket `[]` property binding syntax to pass value from container to child component. In addition, we use `@Input` decorator to annotate the class property in the child component.

```
// pass a value to child component
<pm-star [rating]='product.starRating'></pm-star>

export class StarComponent implements OnChanges {
  // dictate the value is passed from container using @Input()
  @Input() rating: number;
}
```

#### Passing values from child component to container

It's a bit complcated to pass something from the child component to its parent.

In the child component, we need to emit an event using the `@Output()` decorator.

```
export class StarComponent implements OnChanges {
  // define an event emitter
  @Output() ratingClicked: EventEmitter<number> = new EventEmitter();

  // when the component is clicked, emit an event with a payload of number type
  onClick(): void {
    this.ratingClicked.emit(this.rating);
  }
}
```

In the container, we need to listen the event.

```
<pm-star
        [rating]='product.starRating'
        // listen the ratingClicked event from the child component
        // when an ratingClicked event is emitted, onRatingClicked() method is executed
        // $event is the payload emitted from the child component
        (ratingClicked)='onRatingClicked($event)'></pm-star>
        
// handle the event in the container
// the 'message' parameter is the $event payload coming from the child component
onRatingClicked(message: string): void {
  this.pageTitle = `This product is ${message} stars`;
}        
```

#### Passing an HTML element from the view to a component class

Use hash `#` to bind an HTML element to a local variable.

```
<div class="field">
  <label for="link">Link:</label>
  // bind the input element to a variable named 'newlink'
  <input name="link" #newlink>
  // passing the link HTML element to a function
  <button (click)="addArticle(newlink)" class="ui positive right floated button">
    Submit link 
  </button>  
</div>
```
In the component, we receive the HTML element passed in as an HTMLInputElement type.

```
addArticle(link: HTMLInputElement): boolean {
  console.log(`Adding article link: ${link.value}`);
  return false; 
}
```

#### Add Bootstrap to Angular project

Solution 1

Add the Bootstrap CDN link to the root template of an Angular project (index.html).

Solution 2

1. Install Bootstrap using a package manager like NPM or Yarn.
2. Add Bootstrap to the "styles" section in .angular-cli.json.

```
"styles": [
  "../node_modules/bootstrap/dist/css/bootstrap.min.css",
  "styles.css"
],
```

#### Property binding and string interpolation

We use `[property]` to specify property binding and `{{ expression }}` to signal a string interpolation. Both property binding and interpolation binds a component class property to the template, which is one-way binding.

```
// property binding
<img *ngIf="showImage"
     [src]="product.imageUrl"
     [title]="product.productName"
     [style.width.px]="imageWidth"
     [style.margin.px]="imageMargin">
     
<button [disabled]="allowNewServer">Add Server</button>

// string interpolation
<p>Server with ID {{ serverId }} is {{ getServerStatus() }}</p>
```

#### Event binding

We use `(event)` to indicate an event binding. Remember to add a pair of parens after the method name.

```
<button 
  [disabled]="!allowNewServer"
  // The $event parameter is an event object associated with the event being fired.
  (click)="increseServerid($event)"
>Add Server</button>
```

#### Two-way binding

We use `[(ngModel)]` to achieve two-way binding, which is part of the `FormsModule`. So in order to use ngModel, we need to first import it in your Angular module file (for example, app.module.ts).

```
// any change to the input value will also reflect in the component class
<input type="text" [(ngModel)]="listFilter" />
```

#### Directives

```
Structual directives

/* 
The * prefix indicates it is a structural directive,
which means it would change DOM structure based on the condition
*/

<table class="table" *ngIf="products && products.length">
  // other code...
</table>

<tr *ngFor="let product of products">
  // other code...
</tr>
```

#### Pipes

We can use a pipe with the `|` character.

```
// use chain pipes in string interpolation
{{ product.price | currency | lowercase }}

// use parameters in pipe
{{ product.price | currency:'USD':true:'1.2-2' }}

// use pipe in property binding
<img [src]='product.imageUrl' [title]='product.productName | uppercase' />
```

#### Component lifecycle hooks

- OnInit: perform component initialization or retrieve data from backend server
- OnChanges: perform action after change to @input() properties
- OnDestroy: perform cleanup

To use lifecycle hook, we need to import and implement its interface.

```
// import the OnInit interface
import { Component, OnInit } from "@angular/core";

// implement the interface
export class ProductListComponent implements OnInit { ...code }

// define the lifecycle hook method
ngOnInit(): void {
  console.log('ngOnInit');
}
```

#### Services

Define a service

```
import { Injectable } from '@angular/core';
import { IProduct } from './product';

@Injectable()
export class ProductService {
  // define a method
  getProducts(): IProduct[] {}
}
```

Register the service in the closest ancestor component

```
@Component({
  selector: "pm-root",
  templateUrl: "./app.component.html",
  providers: [ProductService]
})
```

Inject the service into a component

```
import { ProductService } from './product.service';

// define a private variable '_productService' and assign it a ProductService instance
constructor(private _productService: ProductService) {
  this.listFilter = '';
}
```

## Angular CLI

#### Create a component

```
ng generate component componentName

ng g c componentName
```
