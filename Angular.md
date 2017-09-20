## Angular

#### Passing values from parent to child component

Use bracket `[]` syntax to pass value from parent to child.

```
<p *ngFor="let name of names">
  // pass a value to the input named foo in <app-user-item> component
  <app-user-item [foo]="name"></app-user-item>
</p>
```
In the child component, use `@Input()` annotation to indicate 

#### Passing an HTML element to a function

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
In the function, we receive the argument passed as an HTMLInputElement type.

```
addArticle(link: HTMLInputElement): boolean {
  console.log(`Adding article link: ${link.value}`);
  return false; 
}
```

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
