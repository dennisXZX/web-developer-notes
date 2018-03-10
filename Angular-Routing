## Angular Routing

#### Routing Flow with Resolver

1. User clicks the link.
2. Angular executes a resolver and returns a value or observable.
3. You can collect the returned value or observable in constructor or in ngOnInit, in class of your component which is about to load.
4. Use the collected the data for your purpose.
5. Now you can load your component.

Resolver is some intermediate code, which can be executed when a link has been clicked and before a component is loaded.

#### Styling Active Links

Add `routerLinkActive="cssClassName"` in the component view where a `[routerLink]` is located, so when the link is active, the CSS class would be applied.

By default, the routerLinkActive would do a start with match, add `[routerLinkActiveOptions]="{exact: true}"` if you want to perform an exact match.

#### Routing

1. Define all the routes (app-routing.ts)
2. define a routing module (app-routing.module.ts)
2. Import the routes in NgModule
3. Declare a <router-outlet> to display the routed components
4. Add [routerLink]="events" bindings to navigate between routes

In order to use routing in Angular, we need to first configure a route file.

```ts
// app-routing.ts
export const appRoutes: Routes = [
  { path: 'events', component: EventsListComponent },
  { path: 'events/:id', component: EventDetailsComponent },
  // pathMatch can be set to 'full' or 'prefix'
  { path: '', pathMatch: 'full', redirectTo: '/events' }
];
```

Then we define a routing module.

```
// app-routing.module.ts
import { NgModel } from "@angular/forms";
import { RouterModule, Routes } from "@angular/router";

// import routes
import { routes } from './app-routing.module';

@NgModel({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }

// define all the components for routing
export const routableCompoents = [
  CharacterListComponent,
  VehicleListComponent
];
```

Next we import the routes in `app.module`.

```ts
import { AppRoutingModule, routableComponents } from './app-routing.module';

@NgModule({
  imports: [
    BrowserModule,
    AppRoutingModule // import AppRoutingModule
  ],
  declarations: [
    AppComponent,
    routableComponents // import all components for routing
  ]
}
```

We need to add a `<base href=''>` tag to index.html to tell Angular the relative path to parse the route.

In the view, we need to use `[routerLink]` to specify a route, and `<router-outlet>` directive to specify where to display the routed component's view.

```ts
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

You can navigate to a route in code using the navigate() method.

```ts
export class CreateEventComponent {
  constructor(private router: Router) {

  }

  cancel() {
    // navigate to '/events' route
    this.router.navigate(['/events']);
  }
}
```
