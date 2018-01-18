## Redux

#### Basic Workflow of Redux

1. create an action type config file for all the action constants
2. create an initial state object for each reducer as a default state (optional)
3. create a bunch of reducers, in each reducer we need to provide logic for handling each action type (usually in a switch statement). We should not directly mutate state in reducers. Also keep in mind that all reducers get called regardless of what action is emitted, so we have to return the original state if an action is not applicable.
4. combine all the reducers into one root reducer
5. create a store which accepts the root reducer as the first parameter
6. subscribe to the store, so a callback is executed when any state is changed
7. create an action creator for each reducer
8. dispatch an action from the store

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
5. pass the root store to the `<Provider>` component, so all the child components can access the state 

```js
import { createStore } from 'redux'
import reducer from './store/reducer';
import { Provider } from 'react-redux';
import * as actionTypes from './actions';

const store = createStore(reducer);

ReactDOM.render(
  <Provider store={store}>
    <APP />
  </Provider>, 
  document.getElementById('root'));
```

1. define `mapStateToProps()` to map your state to props from within the React component
2. define `mapDispatchToProps()` to map your action to props from within the React component 
3. use `connect()` provided by react-redux to connect React and Redux

```js
import { connect } from 'react-redux';

class Counter extends Component {
  ... code
}

// map state to props, the state parameter here refers to the Redux state you defined in your reducers
// because we use combineReducers so we need to access state via the key of the state, such as state.users.counter
// now you can access the Redux state via the this.props.counter from within the React component
const mapStateToProps = (state) => {
  return {
    counter: state.users.counter
  }
}

// map dispatch to props, the dispatch parameter here refers to the dispatch method provided by Redux, so when you call this dispatch method in your code, under the hood it would call the dispatch method attached to the Redux store
// now you can dispatch an action using this.props.onIncrementCounter from within the React component
const mapDispatchToProps = (dispatch) => {
  return {
    onIncrementCounter: () => dispatch({ type: actionType.ADD, payload: 10 })
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```

#### Connect dispatch to props

There are three different ways to connect dispatch to props.

```js
// manually dispatch the action
function mapDispatchToProps = (dispatch) => ({
  requestEmployees: () => dispatch(requestEmployees())
});
```

```js
// using bindActionCreators() helper function
function matchDispatchToProps(dispatch) {
  return bindActionCreators({ editLabResult: requestEmployees}, dispatch);
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

By default, Redux action creator cannot execute asynchronous code as it can only returns an object, so we need to use a helper library `redux-thunk` for the job.

```js
// import redux-thunk
import thunk from 'redux-thunk';

// apply redux-thunk middle to the store
const store = createStore(
  rootReducer,
  applyMiddleware(thunk)
);

// use redux-thunk in action creator
// now this action creator returns a function instead of an object, the 'dispatch' parameter gives you access to the dispatch() store method
export const initIngredients = () => {
  return (dispatch) => {
    axios.get('https://react-burger-app-29cd2.firebaseio.com/ingredients.json')
      .then((response) => {
        dispatch(setIngredients(response.data));
      })
      .catch((error) => {
        dispatch(fetchIngredientsFailed());
      });
  }
};
```
