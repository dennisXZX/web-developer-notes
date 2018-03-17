## Angular

#### How Angular app is bootstrapped

In `.angular-cli.json`, the 'main' property specifies where to look for the bootstrap file, which is `main.ts` by default. In main.ts, an Angular module is specified to bootstrap the app, which is `AppModule` by default. In `app.module.ts`, we specify which component to use as the top-level component in 'bootstrap' property, which is `AppComponent` by default. Also in this file, we specify what other components belong to this module in 'declarations' property.

#### Angular Modules

A module is a mechanism to group components, directives, pipes and services that are related, in such a way that can be combined with other modules to create an application.

Create a feature module:

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterModule } from '@angular/router';

// import components for this module
import { ProfileComponent } from './user-profile/user-profile.component';

// import routes
import { userRoutes } from './user.routes';

@NgModule({
  imports: [
    CommonModule, // use CommonModule instead of BrowserModule
    RouterModule.forChild(userRoutes) // use forChild instead of forRoot
  ],
  declarations: [
    ProfileComponent
  ],
  providers: [

  ]
})
export class UserModule {

}
```

Create the routes for the feature module:

```
import { ProfileComponent } from './user-profile/user-profile.component';

// notice the route for this is not /profile, it is user/profile
// the 'user' path is set in the root module routing config file
export const userRoutes = [
  { path: 'profile', component: ProfileComponent }
]
```

Add the feature module to the root module routing config.

```
export const appRoutes: Routes = [
  { path: 'events/new', component: CreateEventComponent, canDeactivate: [EventRouteActivator] },
  { path: 'events', component: EventsListComponent, resolve: { events: EventListResolver } },
  { path: 'events/:id', component: EventDetailsComponent, canActivate: [EventRouteActivator] },
  { path: '404', component: Error404Component },
  { path: '', redirectTo: '/events', pathMatch: 'full' },
  // specify when the user hits 'user' path, lazy load the UserModule
  // loadChildren accepts a string with two parts, the first part specifies the module path, the second part is the module name
  { path: 'user', loadChildren: 'app/user/user.module#UserModule' }
];
```

#### Angular barrel

An Angular barrel is an ES2015 module file that re-exports selected exports of other ES2015 modules.

Create a barrel:

```
// components
export * from './create-event/create-event.component';
export * from './event-thumbnail/event-thumbnail.component';
export * from './events-list/events-list.component';
export * from './event-details/event-details.component';

// services
export * from './events-list/event-list-resolver.service';
export * from './event-details/event-route-activator.service';
export * from './events-list/event-list-resolver.service';
export * from './shared/event.service';
```

Use the barrel:

```
import {
  EventsListComponent,
  EventThumbnailComponent,
  EventDetailsComponent,
  CreateEventComponent,
  EventRouteActivator,
  EventService,
  EventListResolver
} from './events/index';
```

#### Angular in memory web api

You can fake a server in Angular by using the angular-in-memory-web-api.

Create a file `in-memory-data.service.ts`.

```ts
import { InMemoryDbService } from 'angular-in-memory-web-api';

// createTestCustomers method retrieves the customer data
import { createTestCustomers } from '../test-data';

export class InMemoryDataService implements InMemoryDbService {
  createDb() {
    const states = ['California', 'Illinois', 'Jalisco', 'Quebec', 'Florida'];
    return { customers: createTestCustomers(), states };
  }
}
```

Register it in `app.module`.

```
import { InMemoryWebApiModule } from 'angular-in-memory-web-api';
import { InMemoryDataService }  from './in-memory-data.service';

@NgModule({
  imports: [
    BrowserModule,
    FormsModule,
    InMemoryWebApiModule.forRoot(InMemoryDataService) // <-- register in-mem-web-api and its data
  ]
})  
```

#### @HostBinding()

@HostBinding() allows us to configure host element (the markup in the parent view) from within the component.

For example, we can bind a css class to the host.

```ts
export class ArticleComponent implements OnInit {
  @HostBinding('attr.class') cssClass = 'row';
}
```

So instead of adding the class attribute to the markup `<app-article class="row"></app-article>`, the markup `<app-article></app-article>` would always have a `class="row"` attached to it thanks to the @HostBinding().

#### Safe navigation operator

Angular provides a safe navigation operator `?.` to guard against unexpected property access.

```ts
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

```ts
// pass a value to 'rating' proerty from container
<pm-star [rating]='product.starRating'></pm-star>

export class StarComponent implements OnChanges {
  // dictate the value is passed from container using @Input() decorator
  @Input() rating: number;
  // we can also use an alias, so now outside this component we pass the value using [pageRating]='product.starRating'
  // but inside the component, we use the 'rating' property to store the value
  // the property name (rating) represent how that incoming property will be visible in the controller
  // the @Input argument (pageRating) configures how the property is visible to the outside world
  @Input('pageRating') rating: number;
}
```

#### Passing values from child component to container

It's a bit complcated to pass something from the child component to its parent.

In the child component, we need to emit an event using the `@Output()` decorator.

```js
export class StarComponent implements OnChanges {
  // define an event emitter and specify what type of data the it would emit
  @Output() ratingClicked: EventEmitter<number> = new EventEmitter();

  // when the component is clicked, emit an event with a payload of number type
  // you can only emit a single value at a time, if you need to emit multiple values, you need to wrap them into an object
  onClick(): void {
    this.ratingClicked.emit(this.rating);
  }
}
```

In the container, we need to listen to the event emitting.

```ts
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

```ts
// bind the <event-thumbnail> directive so any public properties and methods of it can be accessed in the template
<event-thumbnail #thumbnail></event-thumbnail>
<h3>{{ thumbnail.someProperty }}</h3>
<button (click)="thumbnail.someMethod()">Click</button>
```

You can also bind a DOM element

```ts
// bind the input element to a variable named 'newlink'
<input name="link" #newlink>
// passing the link HTML element to a function
<button (click)="addArticle(newlink)">Submit</button>  
```

In the component, we receive the HTML element passed in as an `HTMLInputElement` type.

```js
addArticle(link: HTMLInputElement): boolean {
  console.log(`Adding article link: ${link.value}`);
  return false; 
}
```

#### @ViewChild and @ContentChild

You can access an HTML element in the Typescript code using `@ViewChild` decorator.

```ts
// define a template reference variable
<input type="text" #serverContent />
```

```ts
// in Typescript, you get an element reference
@ViewChild('serverContent') serverContent: ElementRef;

// now you can access the value of the input in ngAfterViewInit() life cycle method
this.serverContent.nativeElement.value;
```

If you add a template reference variable to an HTML element between a directive, and you want to access it in the code, you can use `@ContentChild` decorator.

```ts
<app-server-element>
  <p #contentParagraph></p>
</app-server-element>
```

Now you can access the element reference in ngAfterContentInit() life cycle method.

```ts
@ContentChild('contentParagraph') paragraph: ElementRef;
```

#### Add Bootstrap and jQuery to Angular project

Solution 1

Add the Bootstrap and jQuery CDN link to the root template of an Angular project (index.html).

Solution 2

1. Install Bootstrap and jQuery using a package manager like NPM or Yarn.
2. Add Bootstrap to the 'styles' section in .angular-cli.json.
3. Add jQuery to the 'scripts' section in .angular-cli.json.

```ts
"styles": [
  "../node_modules/bootstrap/dist/css/bootstrap.min.css",
  "styles.css"
],
"scripts": [
  "../node_modules/jquery/dist/jquery.min.js",
]
```

4. If you like to have a better theme, replace the default `bootstrap.min.css` file by one of those from [Bootswatch](https://bootswatch.com/).

Solution 3

Add Bootstrap to styles.css

```
@import "~bootstrap/dist/css/bootstrap.css";
```

#### Property binding and string interpolation

We use `[property]` to specify property binding and `{{ expression }}` to signal a string interpolation. Both property binding and interpolation binds a component class property to the template, which is `one-way binding`.

```ts
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

#### Attribute binding

Most of the HTML attribute have a one-to-one mapping to DOM property, but there are a few exceptions. `colspan` is one of a few HTML attributes that does not have a counterpart in DOM property. In this case, we need to use attribute binding.

```ts
<td [attr.colspan]="colSpan"></td>
```

```ts
export class CourseComponent {
  colSpan = 2;
}
```

#### Class and Style binding

```html
<button class="btn btn-primary" [class.active]="isActive">Submit</button>
<button [style.backgroundColor]="isActive ? 'blue' : 'white'">Submit</button>
```

```ts
export class CourseComponent {
  isActive = false;
}
```

`ngClass` can bind multiple classes to an element while `ngStyle` can bind multiple styles.

```ts
// normally we define a function in the component to minimize the logic in the template
<div [ngClass]="{green : event?.time === '8:00 am'}">Time: {{event?.time}} {{timeLabel}}</div>

<div [ngStyle]="{'color' : event?.time === '8:00 am' ? '#003300' : 'normal' }">Time: {{event?.time}} {{timeLabel}}</div>
```

#### Event binding

We use `(event)` to indicate an event binding. Remember to add a pair of parens after the method name.

```ts
<button 
  [disabled]="!allowNewServer"
  // The $event parameter is an event object associated with the event being fired.
  (click)="increseServerid($event)"
>Add Server</button>
```

```ts
export class CourseComponent {
  increseServerid($event) {
    // prevent event bubbling
    $event.stopPropagation();
    
    // other code...
  }
}
```

Angular also has a feature called event filtering.

```
<input (keyup.enter)="onKeyUp()" />
```

```
export class CourseComponent {
  // instead of using
  onKeyUp($event) {
    if ($event.keyCode === 13) {
      console.log('Enter is pressed');
    }
  }
  
  // event filtering can save you some logics to determine which key is pressed
  onKeyUp() {
    console.log('Enter is pressed');
  }
}
```

#### Two-way binding

We use `[(ngModel)]` to achieve two-way binding, which is part of the `FormsModule`. So in order to use ngModel, we need to first import it in your Angular module file (for example, app.module.ts).

```ts
// any change to the input value will also reflect in the component class
<input type="text" [(ngModel)]="listFilter" />
```

#### Structual directives

The * prefix indicates it is a structural directive, which means it would change DOM structure based on the condition

*ngIf

```ts
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

```ts
<tr *ngFor="let product of products; index as i;">
  // other code...
</tr>

// by default, *ngFor tracks object by their memory location
// you can change this behavior by using the trackBy property to reduce resource consumption
<tr *ngFor="let product of products; index as i; trackBy: trackCourse">
  // other code...
</tr>

// define the trackCourse function in the controller
trackCourse(index, course) {
  return course ? course.id : undefined;
}
```

*ngSwitch

```ts
<div [ngSwitch]="event?.time">
  Time: {{event?.time}}
  <span *ngSwitchCase="'8:00 am'">(Early Start)</span>
  <span *ngSwitchCase="'10:00 am'">(Late Start)</span>
  <span *ngSwitchDefault>(Normal Start)</span>
</div>
```

#### Attribute directives

We can create a directive by tag, attribute and class.

```ts
@Component({
  selector: 'app-server',  // by tag
  selector: '[app-server]' // by attribute
  selector: '.app-server'  // by class
})
```

#### Content projection

`<ng-content></ng-content>` can project any HTML code between your custom directives into your component.

We define a custom directive `collapsible-well`.

```html
<div class="row" *ngFor="let session of sessions">
  <div class="col-md-12">
    <collapsible-well [title]="session.name">
      <h4>{{session.name}}</h4>
      <h6>{{session.presenter}}</h6>
      <span>Duration: {{session.duration}}</span><br />
      <span>Level: {{session.level}}</span>
      <p>{{session.abstract}}</p>
    </collapsible-well>
  </div>
</div>
```

In the template of `collapsible-well`, we use `<ng-content></ng-content>` to project content into it.

```html
<div (click)="toggleContent()" class="well pointable">
  <h4 class="well-title">{{ title }}</h4>
  <ng-content></ng-content>
</div>
```

You can also take advantage of multiple slot content projections. The following example projects a title and body section into `collapsible-well`.

```html
<collapsible-well>
  <div well-title>
    {{ session.name }}
    <i *ngIf="session.voters.length > 3" class="glyphicon glyphicon-fire" style="color: red"></i>
  </div>

  <div well-body>
    <h6>{{session.presenter}}</h6>
    <span>Duration: {{session.duration}}</span><br />
    <span>Level: {{session.level}}</span>
    <p>{{session.abstract}}</p>
  </div>
</collapsible-well>
```

In the `collapsible-well` template, you specify an attribute selector to map to the projected content. 

```html
<div (click)="toggleContent()" class="well pointable">
  <h4>
    <!-- [well-title] is an attribute selector -->
    <ng-content select="[well-title]"></ng-content>
  </h4>

  <ng-content select="[well-body]" *ngIf="visible"></ng-content>
</div>
```

#### Pipes

We can use a pipe with the `|` character.

```ts
// use chain pipes in string interpolation
{{ product.price | currency | lowercase }}

// use parameters in pipe
{{ product.price | currency:'USD':true:'1.2-2' }}

// use pipe in property binding
<img [src]='product.imageUrl' [title]='product.productName | uppercase' />
```

#### Async pipe

Async pipe works well with in the situation where you need to fetch data from a server and display the data in a view template.

```ts
export class VehicleListComponent {
  vehicles: Observable<Vehicle[]>;
  
  // inject the VehicleService service
  constructor(private vehicleService: VehicleService) {}
  
  getVehicles() {
    // retrieve an observable from a service
    this.vehicles = this.vehicleService.getVehicles();
  }
}
```

In the view we can then use the `async pipe`.

```ts
<ul>
  <li *ngFor="let vehicle of vehicles | async">
    {{ vehicle.name }}
  </li>
</ul>
```

#### Custom Pipe

This is an example of a custom pipe `{{ text | summary:10 }}`, which extracts a portion of the text as summary.

```
import { Pipe, PipeTransform } from "@angular/core";

@Pipe({
  name: 'summary'
})
export class SummaryPipe implements PipeTransform {
  // the limit parameter is optional
  transform(value: string, limit?: number) {
    if (!value) {
      return null;
    }

    const actualLmit = (limit) ? limit : 50;

    return value.substr(0, actualLmit) + '...';
  }
}
```

In order to use the custom pipe, you would have to add it to the declarations array in your module.

#### Component lifecycle hooks

- ngOnInit: perform component initialization or retrieve data from backend server
- ngOnChanges: perform action after change to @input() properties
- ngOnDestroy: perform cleanup

To use lifecycle hook, we need to import and implement its interface.

```ts
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

In Angular, a service is just a plain Typescript class.

```ts
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

You can register the service in the closest ancestor component, or in its module.

```ts
// register the service in a component
@Component({
  selector: "pm-root",
  templateUrl: "./app.component.html",
  providers: [ProductService]
})
```

Inject the service into a component

```ts
import { ProductService } from './product.service';

// define a private variable 'productService' and assign it an injected ProductService instance
constructor(private productService: ProductService) {
  this.listFilter = '';
}
```
