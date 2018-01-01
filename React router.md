## React router

#### Set up react router in your project

First you need to wrap your application using `<BrowserRouter>`.

```js
import { BrowserRouter } from 'react-router-dom';

const app = (
  <BrowserRouter>
    <App />
  </BrowserRouter>
)

ReactDOM.render(app, document.getElementById('root'));
```

Then you can use `<Switch>` to parse the URL, the first matched URL will be returned. However, keep in mind that the order of the URLs is important, and you should always place the most specific routes on top of the generic ones.

```js
import { Route, Switch } from 'react-router-dom';

<Switch>
  <Route path="/checkout" component={Checkout} />
  <Route path="/" component={BurgerBuilder} />
</Switch>
```

#### 404 error page

We can rely on the `<Switch>` feature to achieve a 404 page. Since only one route will be matched in a Switch, we can place the 404 error component at the bottom to make sure it would always be matched when no other legitimate routes are matched. 

```js
<Switch>
  <Route exact path="/" component={Home} />
  <Route path="/popular" component={Popular} />
  <Route path="/battle" component={Battle} />
  <Route component={Error404}></Route>
</Switch>
```

#### Redirect component

Rendering a <Redirect> will navigate to a new location. The new location will override the current location in the history stack.
  
```js
render() {
  let summary = <Redirect to="/" />;

  if (this.props.ingredients) {
    const purchasedRedirect = this.props.purchased ? <Redirect to="/" /> : null;

    summary = (
      <Fragment>
        {purchasedRedirect}
        <CheckoutSummary
          ingredients={this.props.ingredients}
          checkoutCancelled={this.checkoutCancelledHandler}
          checkoutContinued={this.checkoutContinuedHandler} />
        <Route
          path={this.props.match.path + '/contact-data'}
          component={ContactData} />
      </Fragment>
    );
  }

  return summary;
}
```

#### Inline rendering

We can use inline rendering to pass props to an component

```js
<Route
  path={this.props.match.path + '/contact-data'}
  render={() => (<ContactData ingredients={this.state.ingredients} />)} />
```

#### Navigate between routes

You can use the `history` object provided by the React router.

```js
// pushe a new entry onto the history stack
this.props.history.push('/checkout');
// replace the current entry on the history stack
this.props.history.replace('/checkout');
// move the pointer in the history stack by -1
this.props.history.goBack();
// move the pointer in the history stack by +1
this.props.history.goForward();
```

#### Pass query parameters

You can pass query parameters to `this.props.location` object by using the search property.

```
<Link
  className='button'
  to={{
    pathname: `${this.props.match.url}/result`,
    search: `?playerOneName=${playerOneName}&playerTwoName=${playerTwoName}`
  }}>
  Battle
</Link>
```

#### Pass props to `history.location` object

```js
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

```js
<Route path={this.props.match.path + '/contact-data'} />

<Link to={this.props.match.url + '/about'}>About</Link>
```

#### Pass `history` and `match` objects to any component

You can get access to the `history` object's properties and the closest <Route>'s `match` via the `withRouter` higher-order component.

```js
import { withRouter } from 'react-router'

class ShowTheLocation extends React.Component { }

// Create a new component that is "connected" to the router
const ShowTheLocationWithRouter = withRouter(ShowTheLocation)
```
