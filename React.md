## React

#### Component reference

Component reference only exists in stateful component.

```
// we define a function which accepts the HTML element (input), and assign it to the 'inputEle' property of the class
// so now you can access this HTML component using this.inputEle
<input ref={(input) => { this.inputEle = input }} value='hello' />
```

```
// make a focus on the input when the component is mounted
componentDidMount() {
  this.inputEle.focus();
}
```

#### Default prop values

```
// Specifies the default values for props:
Person.defaultProps = {
  name: 'Stranger'
};
```

#### Proptypes

To use React proptypes, you need to first install the package `npm i prop-types`.

```
import PropTypes from 'prop-types';

Person.propTypes = {
  click: PropTypes.func.isRequired,
  name: PropTypes.string,
  age: PropTypes.number
}
```

#### Update state that depends on the previous state

Because this.setState() execute asynchronously, so we need to take extra care when our state would be updated in different components. Passing a callback to retrieve the previous state is the best approach to protect the integrity of the component state.

```
this.setState((prevState, props) => {
  return {
    clickCount: prevState.click + 1
  }
});
```

#### High Order Component (HOC)

A high order component is just a React component that wraps another one.

Pattern 1:

```
// create a React functional component
const withClass = (props) => {
  return (
    <div className={props.classes}>
      {props.children}
    </div>
  )
};

export default withClass;
```

```
<WithClass classes={styles.App}>
  ... other React components
</WithClass>
```

Pattern 2:

```
// withClass is just a plain Javascript function which returns a React functional component
const withClass = (WrappedComponent, className) => {
  return (props) => (
    <div className={className}>
      <WrappedComponent {...props} />
    </div>
  )
};

export default withClass;
```

We can call withClass() before exporting the component.

```
export default withClass(Person, classes.Person);
```

#### Fragments

Sometimes you do not want to wrap your JSX in an unnecessary div element, now you can use fragment to achieve this.
  
```
// wrap the content in an empty JSX tag.
<>
  <h1>First Element</h1>
  <h1>Second Element</h1>
</>
```

#### React.PureComponent

The difference between React.Component and React.PureComponent is that React.Component doesn’t implement `shouldComponentUpdate()`, but React.PureComponent implements it with a shallow prop and state comparison.

If your React component’s render() function renders the same result given the same props and state, you can use React.PureComponent for a performance boost in some cases.

```
class Greeting extends React.PureComponent {
  render() {
    ... code
  }
}
```

#### Component lifecycle

- Creation phase: constructor() -> componentWillMount() -> render() -> componentDidMount() -> componentWillUnmount()

- Update phase (triggered by props change): componentWillReceiveProps(nextProps) -> shouldComponentUpdate(nextProps, nextState) -> componentWillUpdate(nextProps, nextState) -> render() -> componentDidUpdate() -> componentWillUnmount()

- Update phase (triggered by state change): shouldComponentUpdate(nextProps, nextState) -> componentWillUpdate(nextProps, nextState) -> render() -> componentDidUpdate() -> componentWillUnmount()

#### Error boundary

Error boundary is a React component that catches JavaScript errors anywhere in its child component tree, logs those errors, and displays a fallback UI instead of the component tree that crashed.

A class component becomes an error boundary if it defines a lifecycle method called `componentDidCatch(error, info)`:

```
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  componentDidCatch(error, info) {
    // Display fallback UI
    this.setState({ hasError: true });
    // You can also log the error to an error reporting service
    logErrorToMyService(error, info);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }
    // render its children property
    return this.props.children;
  }
}
```

Then you can use it as a regular component:

```
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

#### Using CSS Module in create-react-app project

First run `npm run eject` to get access to your webpack config file.

Go to `webpack.config.dev.js` and `webpack.config.prod.js` and add CSS modules feature to css-loader.

```
{
  loader: require.resolve('css-loader'),
  options: {
    modules: true,
    localIdentName: '[path][name]__[local]--[hash:base64:5]'
  }
}
```

Now you can import the CSS file `import styles from './App.css'` and use .title style in your code as if you are dealing with a Javascript object `<div className={styles.title}>test</div>`.

To create a global CSS style, you can add a `:global` prefix such as `:global .post { ... } `, then you can use `className="post"` anywhere in your app and receive that styling.

#### Avoid mutating component states directly

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
