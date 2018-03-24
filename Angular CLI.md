## Angular CLI

```ts
// create an Angular project
ng new projectName

// launch the Angular project and open in your default browser
ng serve -o
ng serve -o --port 4201

// create a component, the --flat flag indicates there is no folder created
ng g c products/product-detail.component --flat

// create a component with no testing spec file
ng g c recipes/recipe-list --spec false

// create a component and add it to a custom module
ng g component -m user.module

// create a service and register it in app.module
ng g s products/product-guard -m app.module

// create a directive
ng g d directiveName

// create a module and register it in app.module
ng g m products/product --flat -m app.module

// check the documentation of a CLI command
ng commandName --help

// run unit test and end-to-end test on your application
ng test | ng e2e
// generate a code coverage report
ng test --code-coverage

// generate production code using, uglify, tree-shaking and AOT (Ahead of Time compilation) to minimize the code
// --base-href specify the root URL of your app
ng build --prod
ng build --target=production --base-href /
```
