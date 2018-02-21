## ngrx

The flow of ngrx goes like this.

Components read states from the store, events in the components trigger actions, which are monitored by both effects and reducers, and finally the reducers return new states to the store.

#### Actions

```ts
export const LOAD_ITEMS = 'LOAD_ITEMS';
export const LOAD_ITEMS_SUCCESS = 'LOAD_ITEMS_SUCCESS';
import { Item } from '../../models/item.model';

export class LoadItemsAction {
  readonly type = LOAD_ITEMS;
  constructor() {}
}

export class LoadItemsSuccessAction {
  readonly type = LOAD_ITEMS_SUCCESS;
  constructor(public payload: Item[]) {}
}

export type Action = LoadItemsAction | LoadItemsSuccessAction
```

```ts
// simple action without a payload
dispatch({ type: 'DECREMENT' });

// action with an associated payload
dispatch({
  type: ADD_TODO, 
  payload: {
    id: 1, message: 
    'Learn ngrx/store', 
    completed: true
  }
})
```

#### Reducers

```ts
// a reducer that returns number
export const counter: Reducer<number> = (state: number = 0, action: Action) => {
  switch(action.type){
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
};
```

#### Project data from store for display

Since the store itself is an observable, we have access to a wide range of native Javascript built-in operations, such as map, filter, reduce... etc. Furthermore, powerful RxJS based observable operators are also available at your disposal.

```ts
// most basic example, get people from state
store.select('people')
  
// combine multiple state slices
Observable.combineLatest(
  store.select('people'),
  store.select('events'),
  (people, events) => { // optional projection function
    // projection here
  }
);
```
