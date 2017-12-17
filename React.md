## React

#### Should not mutate states directly

```
// copy an array in the state using slice() or spread operator
const persons = this.state.persons.splice();
const persons = [...this.state.persons];

// copy an object in the state using spread operator or Object.assign()
const person = {...this.state.persons[personIndex]};
const person = Object.assign({}, this.state.persons[personIndex]);
```

#### Passing method reference with pre-defined parameters to child component

We have a handler method with one expected parameter that need to pass to a child component

```
deletePersonHandler = (personIndex) => {
  // copy the current state using slice() or spread operator
  const persons = this.state.persons.splice();
  const persons = [...this.state.persons];
  
  // remove a person from the array based on its index
  persons.splice(personIndex, 1);
  this.setState({ persons: persons })
}
```

We can use the `bind()` method to pass a pre-defined parameter to child component

```
// use bind() to bind the context as well as the index
{this.state.persons.map((person, index) => {
  return <Person
    deletePerson={this.deletePersonHandler.bind(this, index)}
    key={person.id}
    name={person.name} />
})}
```
