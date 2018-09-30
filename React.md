### React

#### Dump JSON into the DOM

```
<pre>
    <code>{JSON.stringify(this.state, null, 4)}</code>
</pre>
```

#### Debugging tips

1. Highlight the React component in Chrome React Dev Tool

2. Switch to Console tab and type `$r` to access the component

3. Now you can freely interact with the component in whatever ways you want, like `$r.setState()`

#### Control whether a property would be overwritten by props

You can use the spread operator to control whether a property would be affected by props.

```
const props = {
  name: 'dennis',
  className: 'notStay'
}

// the className property on the div would not be affected
<div {...props} className="stayHere" />

// the className property from props will overwrite the className property on the div
<div className="stayHere" {...props} />
```

#### Conditional rendering component

- if else statement

```js
const List = ({ list }) => {
  if (!list) {
    return null;
  }

  if (!list.length) {
    return <p>Sorry, the list is empty.</p>;
  } else {
    return (
      <div>
        {list.map(item => <ListItem item={item} />)}
      </div>
    );
  }
}
```

- Ternary operator

```js
const Item = ({ item, mode }) => {
  const isEditMode = mode === 'EDIT';

  return (
    <div>
      { isEditMode
        ? <ItemEdit item={item} />
        : <ItemView item={item} />
      }
    </div>
  );
}
```

- Logical operator

```js
const LoadingIndicator = ({ isLoading }) => {
  return (
    <div>
      { isLoading && <p>Loading...</p> }
    </div>
  );
}
```

- Switch case operator

Please note that you always have to use the default for the switch case operator. In React a component has always to return an element or `null`.

```js
const Notification = ({ text, state }) => {
  switch(state) {
    case 'info':
      return <Info text={text} />;
    case 'warning':
      return <Warning text={text} />;
    case 'error':
      return <Error text={text} />;
    default:
      return null;
  }
}

// since the component is rendered based on a state, it is the best practice to use propTypes to make sure the integrity of the component
Notification.propTypes = {
   text: React.PropTypes.string,
   state: React.PropTypes.oneOf(['info', 'warning', 'error'])
}
```

- Enums

We define an Enum object and use `[state]` to access its value.

```js
// define an Enum object
const NOTIFICATION_STATES = {
  info: <Info />,
  warning: <Warning />,
  error: <Error />,
};

const Notification = ({ state }) => {
  return (
    <div>
      {NOTIFICATION_STATES[state]}
    </div>
  );
}

```

- HOC (Higher Order Component)

```js
// HOC declaration
function withLoadingIndicator(Component) {
  return EnhancedComponent = ({ isLoading, ...props }) => {
    if (!isLoading) {
      return <Component { ...props } />;
    }

    return <div><p>Loading...</p></div>;
  };
}
  
// Usage
const ListWithLoadingIndicator = withLoadingIndicator(List);

<ListWithLoadingIndicator
  isLoading={props.isLoading}
  list={props.list}
/>
```

#### Component reference

Component reference only exists in stateful component, and should be used scarcely in your project since it is not the recommended React way of doing things.

```js
// we define a function which accepts the HTML element (input), and assign it to the 'inputEle' property of the class
// so now you can access this HTML component using this.inputEle
<input ref={(input) => { this.inputEle = input }} value='hello' />
```

```js
// make a focus on the input when the component is mounted
componentDidMount() {
  this.inputEle.focus();
}
```

#### Default prop values

```js
// Specifies the default values for props:
Person.defaultProps = {
  name: 'Stranger'
};
```

#### Proptypes

To use React proptypes, you need to first install the package `npm i prop-types`.

```js
import PropTypes from 'prop-types';

Person.propTypes = {
  click: PropTypes.func.isRequired,
  name: PropTypes.string,
  age: PropTypes.number
}
```

Proptypes can only be used on dev environment, not production environment. You can use `babel-plugin-transform-react-remove-prop-types` plugin to remove proptypes when building app for production.

#### Update state that depends on the previous state

Because this.setState() execute asynchronously, so we need to take extra care when our state would be updated in different components. Passing a callback to retrieve the previous state is the best approach to protect the integrity of the component state. In addition, you can also ensure this.props from the parent component is correctly appied by passing in the second argument props to the callback function.

```js
this.setState((prevState, props) => {
  return {
    clickCount: prevState.click + props.addition
  }
});
```

#### Higher Order Component (HOC)

A higher order component is just a React component that wraps another one.

Pattern 1:

```js
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

```js
<WithClass classes={styles.App}>
  ... other React components
</WithClass>
```

Pattern 2:

```js
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

```js
export default withClass(Person, classes.Person);
```

#### Fragments

Sometimes you do not want to wrap your JSX in an unnecessary div element, now you can use `React.Fragment` to achieve this. There is a short syntax for declaring fragments `<> ...code </>`.
  
```js
// wrap the content in a React.Fragment
<React.Fragment>
  <h1>First Element</h1>
  <h1>Second Element</h1>
</React.Fragment>
```

#### React.PureComponent

The difference between React.Component and React.PureComponent is that React.Component doesn’t implement `shouldComponentUpdate()`, but React.PureComponent implements it with a shallow prop and state comparison.

If your React component’s render() function renders the same result given the same props and state, you can use React.PureComponent for a performance boost in some cases.

```js
class Greeting extends React.PureComponent {
  render() {
    ... code
  }
}
```

#### Component lifecycle

- Creation phase: 
  1. constructor()
  2. componentWillMount()
  3. render()
  4. componentDidMount()
  5. componentWillUnmount()

- Update phase (triggered by props change): 
  1. componentWillReceiveProps(nextProps)
  2. shouldComponentUpdate(nextProps, nextState)
  3. componentWillUpdate(nextProps, nextState) 
  4. render()
  5. componentDidUpdate() 
  6. componentWillUnmount()

- Update phase (triggered by state change): 
  1. shouldComponentUpdate(nextProps, nextState)
  2. componentWillUpdate(nextProps, nextState) 
  3. render() 
  4. componentDidUpdate()
  5. componentWillUnmount()

#### Error boundary

Error boundary is a React component that catches JavaScript errors anywhere in its child component tree, logs those errors, and displays a fallback UI instead of the component tree that crashed.

A class component becomes an error boundary if it defines a lifecycle method called `componentDidCatch(error, info)`:

```js
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

```js
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

#### Using CSS Module in create-react-app project

First run `npm run eject` to get access to your webpack config file.

Go to `webpack.config.dev.js` and `webpack.config.prod.js` and add CSS modules feature to css-loader.

```js
{
  loader: require.resolve('css-loader'),
  options: {
    modules: true,
    localIdentName: '[path][name]__[local]--[hash:base64:5]'
  }
}
```

Now you can import the CSS file `import styles from './App.css'` and use .title style in your code as if you are dealing with a Javascript object `<div className={styles.title}>test</div>`.

You can also add multiple classes to an element `<div className={[styles.title, styles.highlight].join(' ')}>test</div>`.

To create a global CSS style, you can add a `:global` prefix such as `:global .post { ... } `, then you can use `className="post"` anywhere in your app and receive that styling.

#### Avoid mutating component states directly

```js
// copy an array in the state using slice() or spread operator
const persons = this.state.persons.splice();
const persons = [...this.state.persons];

// copy an object in the state using spread operator or Object.assign()
const person = {...this.state.persons[personIndex]};
const person = Object.assign({}, this.state.persons[personIndex]);
```

#### Passing method reference with pre-defined parameters to child component

We have a handler method with one expected parameter that need to pass to a child component

```js
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

```js
// use bind() to bind the context as well as the index
{this.state.persons.map((person, index) => {
  return <Person
    deletePerson={this.deletePersonHandler.bind(this, index)}
    key={person.id}
    name={person.name} />
})}
```
