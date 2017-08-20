## Angular

#### Add Bootstrap to Angular project

Solution 1

Add the Bootstrap CDN link to the root template of an Angular project.

Solution 2

1. Install Bootstrap using a package manager like NPM or Yarn.
2. Add Bootstrap to the "styles" section in .angular-cli.json.

```
"styles": [
  "../node_modules/bootstrap/dist/css/bootstrap.min.css",
  "styles.css"
],
```

#### Create a component using Angular CLI

```
ng generate component componentName

ng g c componentName
```

#### Property binding and string interpolation

We use `[property]` to specify property binding and `{{ expression }}` to signal a string interpolation.

```
<p [innerText]="allowNewServer"></p>
<button [disabled]="allowNewServer">Add Server</button>

<p>Server with ID {{ serverId }} is {{ getServerStatus() }}</p>
```

#### Event binding

We use `(event)` to indicate an event binding. Remember to add a pair of parens after the method name.

The `$event` parameter is an event object associated with the event being fired.

```
<button 
  [disabled]="!allowNewServer"
  (click)="increseServerid($event)"
>Add Server</button>
```

#### Directives

```
<p *ngIf="!isValid">Username should not exceed 5 characters.</p>
```
