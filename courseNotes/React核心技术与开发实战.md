### React核心技术与开发实战笔记 (王红元)

#### Performance

- Using `style={{ display: 'none' }}` would perform better than `shouldComponentDisplay && <Component />`, because the latter would update the DOM while the former only change CSS.

- Never use `index` as `key` value as this would make the `diff` algorithm on virtual DOMs useless.

- Always use `memo()` on functional components to reduce unnecessary renders.

#### setState()

- By default, setState() is asynchronous, it is designed in this way. If setState() is synchronous, it would result in many render() calls, this would lead to performance issue. Another issue would be in between setState() and render() calls, state and props would be different if setState() is synchronous.

- To make setState() synchronous, you can put it in `setTimeout()` or in native event handler. The reason for this is in source code there are checks on whether setState() should be executed immediately or in batches.

#### How React update DOM

- state or props get updated
- render() is re-executed
- a new virtual DOM is created
- compare old virtual DOM and new one
- patch the difference to DOM

#### Event bus in React

`events` NPM package can be used to pass events between components

#### HOC

HOC is a function that accepts a component as parameter and returns a component.

In order to see HOC in dev tool clearly, we can use `Component.displayName` to explicitly set the component display name.

__Classic Use Cases__

- Add more props to a wrapped component
- Add logic authentication logic

However, since React hooks came out, HOC is out of fashion now.

#### ForwardRef

Functional component does not have an instance (like a class component), so using `ref` on it will not get anything (would return `null`). You will need to use `forwardRef`

```js
// Use forwardRef HOC to get access to ref for functional component
const Profile = forwardRef(function Profile(props, ref) {
  return <p ref={ref}>Profile</p>
})

// Create a ref
this.profileRef = createRef();

<Profile ref={this.profileRef}>
```

However, since React hooks came out, ForwardRef is out of fashion now.

#### Portals

Some time we want to render a component out of parent component or virtual DOM tree (for example, display a pop-up window). See example in `react-armory` repo Modal component.

#### Override create-react-app config

Use `craco` package to override create-react-app config without ejecting.

#### Axios

- Use `axios.create()` to create different Axios instances
- Use `axios.interceptors.request.use()` to intercept any network request

Encapsulate Axios example:

```js
// config.js

const devBaseURL = 'https://dev.com';
const prodBaseURL = 'https://prod.com';

export const BASE_URL = process.env.NODE_ENV === 'development' ?  devBaseURL : prodBaseURL;
```

```js
// request.js

import axios from 'axios'
import { BASE_URL } from './config

const instance = axios.create({
  baseURL: BASE_URL
})

// Set up interceptors
instance.interceptors.request.use();

export default instance
```
