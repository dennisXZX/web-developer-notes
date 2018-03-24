## Angular Testing

#### Unit test

Unit test generally means to test a component __in isolation__, without external resources (file system, database, API endpoints). In Angular, a unit test means to test a component without taking consideration of its template. In addition, fake service and router would be used in unit test. We can use `ng test --code-coverage` to see code coverage.

Testing component functions:

```js
describe('compute', () => {
  it('should return 0 if input is negative', () => {
    // compute(number) returns an integer
    const result = compute(-1);
    expect(result).toBe(0);
  });

  it('should include the name in the message', () => {
    // greet(name) returns a string
    expect(greet('dennis').toContain('dennis'));
  });

  it('should return the supported currencies', () => {
    // getCurrencies() returns an array
    const result = getCurrencies();
    expect(result).toContain('USD');
  });
});
```

Testing component state change:

Normally we follow AAA (Arrange - Act - Assert) pattern.

```js
describe('VoteComponent', () => {
  let component: VoteComponent;
  
  // beforeEach() is run before each test case to set things up
  // afterEach() is run after each test to tear things down
  beforeEach(() => {
    // arrange
    component = new VoteComponent();
  });

  it('should increment totalVotes when upvoted', () => {
    // act
    component.upVote();

    // assert
    expect(component.totalVotes).toBe(1);
  });

  it('should decrement totalVotes when downvoted', () => {
    component.downVote();

    expect(component.totalVotes).toBe(-1);
  });
});
```

Testing form:

```js
describe('TodoFormComponent', () => {
  let component: TodoFormComponent;

  beforeEach(() => {
    component = new TodoFormComponent(new FormBuilder());
  });

  it('should create a form with 2 controls', () => {
    expect(component.form.contains('name')).toBe(true);
    expect(component.form.contains('email')).toBe(true);
  });

  it('should make the name control required', () => {
    const control = component.form.get('name');

    control.setValue('');

    expect(control.valid).toBe(false);
  });
});
```

Testing component that involves services:

```js
describe('TodosComponent', () => {
  let component: TodosComponent;
  let service: TodoService;

  beforeEach(() => {
    service = new TodoService(null);
    component = new TodosComponent(service);
  });

  it('should set todos property with the items returned from server', () => {
    let todos = [
      { id: 1, title: 'a' },
      { id: 2, title: 'b' }
    ];

    // when the getTodos method in TodoService is called
    // this fake function will be executed instead of the getTodos method
    spyOn(service, 'getTodos').and.callFake(() => {
      return Observable.from([ todos ]);
    });

    // execute the ngOnInit() life cycle method
    component.ngOnInit();

    expect(component.todos.length).toBe(todos);
  });
});
```

#### Integration test

In Angular term, an integration test involves testing the component with its template. Fake service is used in this kind of test.

#### End-to-end test

Test whether the flow of an application right from start to finish is behaving as expected.
