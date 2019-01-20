## Redux

#### data structure in Redux store (array vs object)

[This video](https://www.youtube.com/watch?v=aJxcVidE0I0&feature=youtu.be) shows how you can use object data structure in Redux store to dramatically simply CRUD operations.

Using object would make CRUD operations easier, however, it does come with disadvantages.

- You cannot iterate over an object as easy as an array. you might need helper functions from Lodash such as `_.map()` or `_.values()`

- Array can preserve the order of data, but not object. To address this, you might keep a separate array of IDs to denote order.

#### Middleware

Concept: a Redux middleware sits between an action and a reducer. When it intercepts an action that it is interested, it would manipulate it with its own logics. The middeware could stop the action or return a new action which goes through all the middlewares again. (Because middlewares do not run in a particular order, so the new action needs to go through all middlewares again in case it misses some of them)

```js
// boilerplate for the Redux middleware
export default ({ dispatch }) => next => action => {
  ... code
};

// the boilerplate above is equal to the this
export default function (dispatch) {
  return function (next) {
    return function (action) {
      ... code
    }
  }
}
```

A example of Redux-promise middleware.

```js
export default ({ dispatch }) => next => action => {
  // Check to see if the action
  // has a promise on its 'payload' property
  // If it does, then wait for it to resolve
  // If it doesn't, then send the action on to the
  // next middleware
  if (!action.payload || !action.payload.then) {
    return next(action);
  }

  // We want to wait for the promise to resolve
  // (get its data!!) and then create a new action
  // with that data and dispatch it
  action.payload.then((response) => {
    // create a new action
    const newAction = { ...action, payload: response };

    // dispatch the new action
    dispatch(newAction);
  });
};

```

#### Basic Workflow of Redux

- create an action type config file for all the action constants
- create an initial state object for each reducer as a default state (optional)
- apply middleware so each action goes through the middleware before hitting reducer (optional)
- create a bunch of reducers, in each reducer we need to provide logic for handling interested action types (usually in a switch statement). We should return a new piece of state instead of directly mutating state in reducers. Also keep in mind that all reducers get called regardless of what action is emitted, so we have to return the original state if an action is not applicable.
- combine all the reducers into one root reducer
- create a store which accepts the root reducer as the first parameter
- subscribe to the store, so a callback is executed when any state is changed
- create an action creator (which normally returns an object) for each action
- dispatch an action from the store

```js
// actions.js
// create an action type config file
export const SPEAK = 'SPEAK';
export const ADD = 'ADD';
export const DELETE = 'DELETE';
```

```js
import redux from 'redux';
import { createStore, combineReducers } from 'redux';
import * as actionTypes from './actions';

// initialize the state for userReducer
const initialState = {
  data: ''
}

// create a user reducer
const userReducer = (state = initialState, action) => {
  switch (action.type) {
    case actionTypes.SPEAK:
      return {
        ...state,
        data: action.payload
      }
    default:
      return state;
  }
};

// create an item reducer
const itemReducer = (state = [], action) => {
  switch (action.type) {
    case actionTypes.ADD:
      return state.concat(action.payload);
    case actionTypes.DELETE:
      // use the slice() to return a shallow copy of a portion of an array
      // slice(0, -1) extracts the first item through the second-to-last one
      return state.slice(0, -1);
    default:
      return state;
  }
};

// combine all reducers
const appReducer = combineReducers({
  users: userReducer,
  items: itemReducer
});

// create a store and pass in the root reducer
const appStore = createStore(appReducer);

// subscribe to the store, so a callback is executed anytime a piece of state is changed
appStore.subscribe(() => {
  console.log('[Subscription]', appStore.getState());
});

// create action creators
const addItemActionCreator = (item) => {
  return {
    type: actionTypes.ADD,
    payload: item
  }
}

const deleteItemActionCreator = (item) => {
  return {
    type: actionTypes.DELETE
  }
}

const speakUserActionCreator = (item) => {
  return {
    type: actionTypes.SPEAK,
    payload: item
  }
}

// dispatch actions
appStore.dispatch(addItemActionCreator('milk'));
appStore.dispatch(speakUserActionCreator('You are screwed!'));
```

#### Connecting React with Redux (react-redux)

1. create each reducer in its own file and then export it
2. combine all the reducers into a root reducer in an index.js file and then export it
3. create a store which accepts the root reducer as the first parameter
4. wrap the root React component with a `<Provider>` component from react-redux
5. pass the root store to the `<Provider>` component, so all the child components can access the store 

```js
import { createStore } from 'redux'
import rootReducer from './store/rootReducer';
import { Provider } from 'react-redux';
import * as actionTypes from './actions';

const store = createStore(rootReducer);

ReactDOM.render(
  <Provider store={store}>
    <APP />
  </Provider>, 
  document.getElementById('root'));
```

1. define `mapStateToProps()` to map your state to props from within React component
2. define `mapDispatchToProps()` to map your actions to props from within React component 
3. use `connect()` provided by react-redux to connect React and Redux

```js
import { connect } from 'react-redux';

class Counter extends Component {
  ... code
}

// map state to props, the state parameter here refers to the Redux state
// because we use combineReducers so we need to access state via a key of the state, such as state.users.counter
// now you can access the Redux state via this.props.counter from within React component
const mapStateToProps = state => {
  return {
    counter: state.users.counter
  }
}

// map dispatch to props, the dispatch parameter here refers to the dispatch method provided by Redux, 
// so when you call this dispatch method in your code, 
// under the hood it would call the dispatch method provided by the Redux store
// now you can dispatch an action using this.props.onIncrementCounter from within React component
const mapDispatchToProps = dispatch => {
  return {
    // dispatch a plain action object
    onIncrementCounter: () => dispatch({ type: actionType.ADD, payload: 10 })
    // dispatch an action returned from an action creator
    onDecrementCounter: () => dispatch(decrementCounter(10))
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```

#### Connect dispatch to props

There are various ways to connect dispatch to props.

```js
// manually dispatch the action
const mapDispatchToProps = dispatch => ({
  requestEmployees: () => dispatch(requestEmployees())
});
```

```js
// return an object, connect would automatically wrap the function in a dispatch()
// if you need to pass arguments to the action, you can use a wrapper function to do that
const mapDispatchToProps = {
  requestEmployees,
  increment: () => increment(42)  // use a wrapper function to pass an argument
};
```

```js
// using bindActionCreators() helper function
const mapDispatchToProps = dispatch => {
  return bindActionCreators({ requestEmployees }, dispatch);
}
```

```js
// pass the object to connect() and it will do the wrapping for you
connect(mapStateToProps, { requestEmployees: requestEmployees, anotherAction: anotherAction })(Component)
```

#### Update state immutably

It is important to note that we should not mutate our state directly. Each time we need to change our state we need to copy the current state first and then make necessary changes based on that. In ES6, the spread operator `...` can do the trick. But keep in mind that the spread operator only does a shallow copy.

```js
return {
  // use spread operator to copy all the properties on the current state
  ...state,
  counter: state.counter + 1,
  data: action.payload
}
```

- use `concat()` instead of `push()` when adding an item to an array
- use `filter()` instead of `splice()` when deleting an item from an array

When dealing with nested objects, every level of nesting must be copied and updated appropriately. It is a good practice to always keep your state flattened in order to avoid multiple level states copy.

```js
// inserting immutably to an array
function insertItem(array, action) {
  let newArray = array.slice();
  newArray.splice(action.index, 0, action.item);
  return newArray;
}
 
// removing immutably from an array
function removeItem(array, action) {
  let newArray = array.slice();
  newArray.splice(action.index, 1);
  return newArray;
}

// alternative for removing immutably from an array
function removeItem(array, action) {
  return array.filter((item, index) => index !== action.index);
}

// updating an item in an array immutably
function updateObjectInArray(array, action) {
  return array.map((item, index) => {
    if(index !== action.index) {
      // this isn't the item we care about, so keep it as-is
      return item;
    }

    // otherwise, this is the one we want, so return an updated value
    return {
      ...item,
      ...action.item
    };    
  });
}
```

#### Redux-thunk

By default, Redux action creator cannot execute asynchronous code as it can only returns an object, so we need to use a middleware library `redux-thunk` for the job.

```js
// import redux-thunk
import thunk from 'redux-thunk';

// apply redux-thunk middle to the store
const store = createStore(
  rootReducer,
  applyMiddleware(thunk)
);

/*
  use redux-thunk in action creator
  now this action creator returns a function instead of an object, the 'dispatch' parameter gives you access to the dispatch() store method
  every async action should have three states, pending, success and failed
*/

export const requestResults = () => (dispatch) => {
  // get into the pending state when this action is fired
  dispatch({ type: REQUEST_RESULTS_PENDING });

  axios.get('https://react-burger-app-29cd2.firebaseio.com/ingredients.json')
    // dispatch success state if the async call returns results
    .then((response) => {
      dispatch({ type: REQUEST_RESULTS_SUCCESS, payload: response });
    })
    // dispatch failed state if the async call doesn't make it
    .catch((error) => {
      dispatch({ type: REQUEST_RESULTS_FAILED, payload: error });
    });
};
```

In the reducer, we handle all three states accordingly.

```js
const initialState = {
  isPending: false,
  results: [],
  error: ''
}

export const requestResults = (state = initialState) => {
  switch (action.type) {
    case REQUEST_RESULTS_PENDING:
      return {
        ...state,
        isPending: true
      }
    case REQUEST_RESULTS_SUCCESS:
      return {
        ...state,
        results: action.payload,
        isPending: false
      }
    case REQUEST_RESULTS_FAILED:
      return {
        ...state,
        error: action.payload,
        isPending: false
      }
    default:
      return state;
  }
}
```
