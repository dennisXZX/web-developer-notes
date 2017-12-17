## Mobx

#### Using Mobx with React

Since create-react-app by default does not come with Mobx, we need to `npm run eject` and edit the `package.json` file in order to use Mobx in our project.

Install Mobx by `npm i mobx mobx-react` and `npm i -D babel-plugin-transform-decorators-legacy`.

```
"babel": {
  "presets": [
    "react-app"
  ],
  "plugins": [
    "transform-decorators-legacy"
  ]  
},
```

#### Workflow of Mobx

Create a store

```
import { observable, action, computed } from 'mobx';

class TodoStore {
  @observable todos = ["buy milk", "buy eggs"];
  @observable filter = "";
  
  // any method that changes the state should be labelled as @action
  @action addTodo(todo) {
    this.todos.push(todo);
  }
  
  // any time the property of todos changes, the @computed will be executed
  @computed get todoCount() {
    return this.todos.length;
  }
}

// create a new store
const store = new TodoStore();

export default store;
```

Use the store in a React component

```
import TodoStore from './TodoStore';

ReactDOM.render(
  <TodoList TodoStore={TodoStore} />, 
  document.getElementById('root')
);
```

Access values in the store

```
import { inject, observer } from 'mobx-react';

// inject the TodoStore and StudentStore into TodoList component and observe any @observable changes
@inject('TodoStore', 'StudentStore')
@observer
class TodoList extends Component {
  render() {
    return <h1>{this.props.TodoStore.todos[0]}</h1>
  }
}
```
