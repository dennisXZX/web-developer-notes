## Angular Forms

#### Reactive form

For reactive form, you first need to initialize all the fields in the component class.

```ts
// initialize two input fields
ngOnInit() {
  const firstName = new FormControl(
    this.authService.currentUser.firstName,
    Validators.required
  );
  const lastName = new FormControl(
    this.authService.currentUser.lastName,
    Validators.required
  );

  this.profileForm = new FormGroup({
    firstName: firstName,
    lastName: lastName
  });
}
```

Then you bind the form group and form control to the template.

```html
// [formGroup]="profileForm" to bind the form group
// formControlName="firstName" to bind the form control
// (ngSubmit)="saveProfile(profileForm.value)" pass the form values to component class
<form [formGroup]="profileForm"
      (ngSubmit)="saveProfile(profileForm.value)"
      autocomplete="off" novalidate>

  <!-- first name field -->
  <div class="form-group" [ngClass]="{'error':
  profileForm.controls.firstName.invalid && profileForm.controls.firstName.touched}">
    <label for="firstName">First Name:</label>
    <input formControlName="firstName" id="firstName" type="text" class="form-control" placeholder="First Name..." />
  </div>
  
  <button type="submit" class="btn btn-primary">Save</button>
 </form>
```

You can validate the form field by passing in validators to the form control.

```ts
this.firstName = new FormControl(
  this.authService.currentUser.firstName,
  [Validators.required, Validators.pattern('[a-zA-Z].*')]
);
```

```
<span class="warning-message"
      // validate in the view
      *ngIf="!validateFirstName() && profileForm.controls.firstName.errors.pattern">
  Must start with a letter
</span>
```

#### Template-based form

`(ngModel)="userName"` binds the input value to the form, which can be accessed by using `<form #loginForm="ngForm" autocomplete="off" (ngSubmit)="login(loginForm.value)">`. Here `loginForm.value` will contain an object of each input field.

In order to use the ngModel directive, we need to import `FormsModule` in the module.

```html
<h1>Login</h1>
<hr>
<div class="col-md-4">
  <form #loginForm="ngForm" autocomplete="off" (ngSubmit)="login(loginForm.value)">
    <div class="form-group" >
      <label for="userName">User Name:</label>
      <input (ngModel)="userName" name="userName"
        id="userName" type="text" class="form-control" placeholder="User Name..." />
    </div>
    <div class="form-group" >
      <label for="password">Password:</label>
      <input (ngModel)="password" name="password"
        id="password" type="password" class="form-control"placeholder="Password..." />
    </div>

    <button type="submit" class="btn btn-primary">Login</button>
    <button type="button" class="btn btn-default">Cancel</button>
  </form>
</div>
```

We can then consume the data from the form, which is passed to the component class in the form of an object.

```
login(formValues) {
  // call the authService to authenticate user
  this.authService.loginUser(formValues.userName, formValues.password);
  this.router.navigate(['events']);
}
```

#### touched vs dirty

The difference between `firstname.touched` and `firstname.dirty` is that when your cursor moves in a field and does nothing then moves out, it is 'touched' but not 'dirty'. But when you types something in the field, it is considered 'dirty'. 
