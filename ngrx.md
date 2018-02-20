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
