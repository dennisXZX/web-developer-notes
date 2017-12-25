## React-router

#### Set up react-router in your project

First you need to wrap your application using `<BrowserRouter>`.

```
import { BrowserRouter } from 'react-router-dom';

const app = (
  <BrowserRouter>
    <App />
  </BrowserRouter>
)

ReactDOM.render(app, document.getElementById('root'));
```

Then you can use `<Switch>` to parse the URL, the first matched URL will be returned. However, keep in mind that the order of the URLs is important, and you should always place the most specific routes on top of the generic ones.

```
import { Route, Switch } from 'react-router-dom';

<Switch>
  <Route path="/checkout" component={Checkout} />
  <Route path="/" component={BurgerBuilder} />
</Switch>
```

#### Inline rendering

We can use inline rendering to pass props to an component

```
<Route
  path={this.props.match.path + '/contact-data'}
  render={() => (<ContactData ingredients={this.state.ingredients} />)} />
```

#### Navigate between routes

You can use the `history` object provided by the React router.

```
// pushe a new entry onto the history stack
this.props.history.push('/checkout');
// replace the current entry on the history stack
this.props.history.replace('/checkout');
// move the pointer in the history stack by -1
this.props.history.goBack();
// move the pointer in the history stack by +1
this.props.history.goForward();
```

#### Pass props to `history.location` object

```
// pass extra props to the 'location' object
this.props.history.push({
  pathname: '/template',
  search: '?query=abc',
  state: { detail: response.data }
});

// then in the component which is rendered with /template route, you can access the props passed like
this.props.location.state.detail
```

#### Get the current path or URL

You can use the `match` object to retrieve the current path or URL. 

```
<Route path={this.props.match.path + '/contact-data'} />

<Link to={this.props.match.url + '/about'}>About</Link>
```

#### Pass `history` and `match` objects to any component

You can get access to the `history` object's properties and the closest <Route>'s `match` via the `withRouter` higher-order component.

```
import { withRouter } from 'react-router'

class ShowTheLocation extends React.Component { }

// Create a new component that is "connected" to the router
const ShowTheLocationWithRouter = withRouter(ShowTheLocation)
```
