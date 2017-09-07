## Redux

```
import { createStore, combineReducers } from 'redux';

// create a user reducer
const userReducer = (state = {}, action) => {
    switch (action.type) {
        case 'SPEAK':
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
        case 'ADD':
            return state.concat(action.payload);
        case 'DELETE':
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

// create a store
const appStore = createStore(appReducer);

// retrieve the current state from the store
console.log('App store created: ',  appStore.getState());

// create action creators
const addItemActionCreator = (item) => {
    return {
        type: 'ADD',
        payload: item
    }
}

const deleteItemActionCreator = (item) => {
    return {
        type: 'DELETE'
    }
}

const speakUserActionCreator = (item) => {
    return {
        type: 'SPEAK',
    }
}

// dispatch an action
appStore.dispatch(addItemActionCreator('milk'));

// retrieve the state from the store
console.log('After dispatching an action: ',  appStore.getState());
```
