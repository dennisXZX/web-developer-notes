## Aurelia

#### Fundamentals

`repeat.for` to generate HTML based on an array, map or set.

`HTML_attribute.bind` to bind any HTML attribute to its view model.

`HTML_event.trigger` to register a method upon firing an event.

`css="className: ${todo.done ? 'line-through' : 'none'}"` to conditionally apply a CSS class.

```
<template>
    <h1>${heading}</h1>

    <form submit.trigger="addTodo()">
        <input type="text" value.bind="todoDescription">
        <button type="submit">Add Todo</button>
    </form>

    <ul>
        <li repeat.for="todo of todos">
            <input type="checkbox" checked.bind="todo.done">
            <span css="text-decoration: ${todo.done ? 'line-through' : 'none'}">
            ${todo.description}
          </span>
            <button click.trigger="removeTodo(todo)">Remove</button>
        </li>
    </ul>
</template>
```
