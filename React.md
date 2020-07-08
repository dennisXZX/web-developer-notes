### React

#### Performance optimisation

First we use Profiler from React dev tool to look into what components are having performance issue.

- To address wasted rendering issue:

For class component, we can convert components into pure components. Note that if your component accepts an arrow function as a prop like `onClick={() => setIsOpen(true)}`, this would not prevent the component from re-rendering even it is a pure component. Because every time the component is rendered, a new function is passed to it, so pure component alone would not do the job. You will need to use `useCallback` hook to create a function and pass to the component, so each render the function would be the same reference. `const showDialog = useCallback(() => setIsOpen(true))`.

We can also implement `shouldComponentUpdate(nextProps, nextState)` to control when a class component should be rendered.

For functional component, we can rely on `React.memo(MyComponent, comparisonFunction)` to achieve the same result as class component.

- To address expensive operation issue:

Use `useMemo()` hook to cache heavy computed result.

- To reduce bundle size:

Make sure you build your app in `production` build mode.

Lazy load your components using `lazy()` and `<Suspense>`.

#### Higher Order Component (HOC)

Steps to build a HOC:

Image we need to build a HOC which checks whether a user is signed in or not. In addition, we want to apply this login checking logic to all other components that require authentication.

1. write the logic you want to reuse in a component

The logic we need to navigate users based on their login status.

```js
// Our component just got rendered
componentDidMount() {
  this.shouldNavigateAway();
}

// Our component just got updated
componentDidUpdate() {
  this.shouldNavigateAway();
}

shouldNavigateAway() {
  if (!this.props.auth) {
    this.props.history.push('/');
  }
}
```

Access the authentication piece of state in the component.

```js
function mapStateToProps(state) {
  return { auth: state.auth };
}
```

2. create a HOC file and add the HOC scaffold

```js
// define a function that takes a Component that need to be enhanced
const withAuth = WrappedComponent => {
  // define a class that renders the component received as parameter
  class EnhancedComponent extends Component {
    render() {
      return <WrappedComponent />;
    }
  }

  return EnhancedComponent;
};
```

3. move the reusable logic into the HOC and pass props through to child component

```js
import React, { Component } from 'react'
import { connect } from 'react-redux'

// define a function that takes a component that need to be enhanced
const withAuth = WrappedComponent => {

  // define a class
  class EnhancedComponent extends Component {
    state = {
      username: 'dennis',
    };
  
    // Our component just got rendered
    componentDidMount () {
      this.shouldNavigateAway()
    }

    // Our component just got updated
    componentDidUpdate () {
      this.shouldNavigateAway()
    }

    shouldNavigateAway () {
      if (!this.props.auth) {
        this.props.history.push('/')
      }
    }

    // pass EnhancedComponent state to wrapped component, so the EnhancedComponent state would appear as props in WrappedComponent component
    // pass props received by EnhancedComponent into WrappedComponent component
    render () {
      return <WrappedComponent {...this.state} {...this.props} />
    }
  }

  // access the state
  function mapStateToProps (state) {
    return { auth: state.auth }
  }

  return connect(mapStateToProps)(EnhancedComponent)
};
```

4. Use the enhanced component

```js
const EnhancedTitle = withAuth(OriginalTitle);

class App extends React.Component {
  render () {
    return <EnhancedTitle config={config} />
  }
}
```

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

Please note that you need to have a default case for switch case operator. In React a component must return either an element or `null`.

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

#### React lifecycle

Here is an interactive React life cycle diagram.

[http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

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
