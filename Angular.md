## Angular

#### How Angular app is bootstrapped

In `.angular-cli.json`, the 'main' property specifies where to look for the bootstrap file, which is `main.ts` by default. In main.ts, an Angular module is specified to bootstrap the app, which is `AppModule` by default. In `app.module.ts`, we specify which component to use as the top-level component in 'bootstrap' property, which is `AppComponent` by default. Also in this file, we specify what other components belong to this module in 'declarations' property.

#### Safe navigation operator

Angular provides a safe navigation operator `?.` to guard against unexpected property access.

```
// the 'product' object might be fetched from an asynchronous call
// so when the template tries to display the productName, the object might still be 'undefined'
// the safe navigation operator only accesses the 'productName' property when the 'product' object is ready
// another way to guard against 'undefined' is to use *ngIf directive
<div class="panel-heading">
  {{ pageTitle + ': ' + product?.productName }}
</div>
```

#### Passing values from container to child component

Use bracket `[]` property binding syntax to pass value from container to child component. In addition, we use `@Input` decorator to annotate the class property in the child component.

```
// pass a value to 'rating' proerty from container
<pm-star [rating]='product.starRating'></pm-star>

export class StarComponent implements OnChanges {
  // dictate the value is passed from container using @Input() decorator
  @Input() rating: number;
  // we can also use an alias, so now outside this component we pass the value using [pageRating]='product.starRating'
  // but inside the component, we use the 'rating' property to store the value
  @Input('pageRating') rating: number;
}
```

#### Passing values from child component to container

It's a bit complcated to pass something from the child component to its parent.

In the child component, we need to emit an event using the `@Output()` decorator.

```
export class StarComponent implements OnChanges {
  // define an event emitter and specify what type of data the it would emit
  @Output() ratingClicked: EventEmitter<number> = new EventEmitter();

  // when the component is clicked, emit an event with a payload of number type
  onClick(): void {
    this.ratingClicked.emit(this.rating);
  }
}
```

In the container, we need to listen to the event emitting.

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

#### Template reference variables

Use hash `#` to bind a DOM element or directive to a local variable, so the variable can be used anywhere in the template (not in your Typescript code). This is often used for parent component to access public properties or methods of child components.

```
// bind the <event-thumbnail> directive so any public properties and methods of it can be accessed in the template
<event-thumbnail #thumbnail></event-thumbnail>
<h3>{{ thumbnail.someProperty }}</h3>
<button (click)="thumbnail.someMethod()">Click</button>
```

You can also bind a DOM element

```
// bind the input element to a variable named 'newlink'
<input name="link" #newlink>
// passing the link HTML element to a function
<button (click)="addArticle(newlink)">Submit</button>  
```

In the component, we receive the HTML element passed in as an `HTMLInputElement` type.

```
addArticle(link: HTMLInputElement): boolean {
  console.log(`Adding article link: ${link.value}`);
  return false; 
}
```

#### @ViewChild and @ContentChild

You can access an HTML element in the Typescript code using `@ViewChild` decorator.

```
// define a template reference variable
<input type="text" #serverContent />
```

```
// in Typescript, you get an element reference
@ViewChild('serverContent') serverContent: ElementRef;

// now you can access the value of the input in ngAfterViewInit() life cycle method
this.serverContent.nativeElement.value;
```

If you add a template reference variable to an HTML element between a directive, and you want to access it in the code, you can use `@ContentChild` decorator.

```
<app-server-element>
  <p #contentParagraph></p>
</app-server-element>
```

Now you can access the element reference in ngAfterContentInit() life cycle method.

```
@ContentChild('contentParagraph') paragraph: ElementRef;
```

#### Add Bootstrap and jQuery to Angular project

Solution 1

Add the Bootstrap and jQuery CDN link to the root template of an Angular project (index.html).

Solution 2

1. Install Bootstrap and jQuery using a package manager like NPM or Yarn.
2. Add Bootstrap to the 'styles' section in .angular-cli.json.
3. Add jQuery to the 'scripts' section in .angular-cli.json.

```
"styles": [
  "../node_modules/bootstrap/dist/css/bootstrap.min.css",
  "styles.css"
],
"scripts": [
  "../node_modules/jquery/dist/jquery.min.js",
]
```

4. If you like to have a better theme, replace the default `bootstrap.min.css` file by one of those from [Bootswatch](https://bootswatch.com/).

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

#### Structual directives

The * prefix indicates it is a structural directive, which means it would change DOM structure based on the condition

*ngIf

```
// *ngIf will add or remove the DOM element based on conditions
<table class="table" *ngIf="products && products.length">
  // other code...
</table>

// you can also add an else block
<div *ngIf="condition; else elseBlock">...</div>
<ng-template #elseBlock>...</ng-template>

// another way to show or hide a DOM element is by using 'hidden' property
// the advantage of this approach is the element is always in the DOM
// if you need to frequently show or hide this element, this perform much better than *ngIf
// as there is no need to re-render the element each time the condition changes
<div [hidden]="!event.price">Price: ${{event.price}}</div>
```

*ngFor

```
<tr *ngFor="let product of products; index as i;">
  // other code...
</tr>
```

*ngSwitch

```
<div [ngSwitch]="event?.time">
  Time: {{event?.time}}
  <span *ngSwitchCase="'8:00 am'">(Early Start)</span>
  <span *ngSwitchCase="'10:00 am'">(Late Start)</span>
  <span *ngSwitchDefault>(Normal Start)</span>
</div>
```

#### Directives

We can create a directive by tag, attribute and class.

```
@Component({
  selector: 'app-server',  // by tag
  selector: '[app-server]' // by attribute
  selector: '.app-server'  // by class
})
```

`ngClass` can bind multiple classes to an element while `ngStyle` can bind multiple styles.

```
// normally we define a function in the component to minimize the logic in the template
<div [ngClass]="{green : event?.time === '8:00 am'}">Time: {{event?.time}} {{timeLabel}}</div>

<div [ngStyle]="{'color' : event?.time === '8:00 am' ? '#003300' : 'normal' }">Time: {{event?.time}} {{timeLabel}}</div>
```

`<ng-content></ng-content>` can project any HTML code between your custom directives into your component.

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

- ngOnInit: perform component initialization or retrieve data from backend server
- ngOnChanges: perform action after change to @input() properties
- ngOnDestroy: perform cleanup

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

// the @Injectable() decorator means this service also depends on other injected services
@Injectable()
export class ProductService {
  // define a method
  getProducts(): IProduct[] {
    code here ...
  }
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

// define a private variable '_productService' and assign it an injected ProductService instance
constructor(private _productService: ProductService) {
  this.listFilter = '';
}
```

#### Routing

In order to use routing in Angular, we need to first configure a route file.

```
export const appRoutes: Routes = [
  { path: 'events', component: EventsListComponent },
  { path: 'events/:id', component: EventDetailsComponent },
  // pathMatch can be set to 'full' or 'prefix'
  { path: '', redirectTo: '/events', pathMatch: 'full' }
];
```

Then we need to import the routes in NgModule.

```
@NgModule({
  imports: [
    BrowserModule,
    RouterModule.forRoot(appRoutes)
  ]
}
```

We need to add a `<base href=''>` tag to index.html to tell Angular the relative path to parse the route.

In the view, we need to use `[routerLink]` to specify a route, and `<router-outlet>` directive to specify where to display the routed component's view.

```
<nav class="navbar navbar-default">
  <div class="container-fluid">
    <a class="navbar-brand">{{ pageTitle }}</a>
    <ul class="nav navbar-nav">
      <li><a [routerLink]="['/welcome']">Home</a></li>
      // go to /events with the event id as a parameter
      <li><a [routerLink]="['/events', event.id]">Event</a></li>
    </ul>
  </div>
</nav>

<div class="container">
  // <router-outlet> specify where to display the routed component's view
  <router-outlet></router-outlet>
</div>
```

You can navigate to a route in code using the navigate method.

```
export class CreateEventComponent {
  constructor(private _router: Router) {

  }

  cancel() {
    // navigate to '/events' route
    this._router.navigate(['/events']);
  }
}
```

## Angular CLI

```
// create an Angular project
ng new projectName

// launch the Angular project and open in your default browser
ng serve -o

// generate production code using, uglify, tree-shaking and AOT (Ahead of Time compilation)  to minimize the code
ng build --prod

// run unit test and end-to-end test on your application
ng test | ng e2e

// create a component, the --flat flag indicates there is no folder created
ng g c products/product-detail.component --flat

// create a component with no testing spec file
ng g c recipes/recipe-list --spec false

// create a service and register it in app.module
ng g s products/product-guard.service -m app.module

// create a module and register it in app.module
ng g m products/product --flat -m app.module

// check the documentation of a CLI command
ng commandName --help
```
