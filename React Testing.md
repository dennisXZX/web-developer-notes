## React Testing

To test in the frontend, we need a suite of toolkit.

- Test framework, which acts as a scaffolding for the testing. (Jasimine, Jest, Mocha)
- Assertion library, which verifies that things are correct. (Jasmine, Jest, Chai)
- Test runner, which runs your tests in browser, headless browser (Puppeteer) or JsDOM. (Jasmine, Jest, Mocha, Karma)
- Spy, Stub and Mock - Spy provides info about your function, Stub replaces a function with a selected function, Mock fakes a function.  (Jasmine, Jest, Sinon)
- Code coverage, which gives you coverage report on your test. (Jest, Istanbul)

In order to test a React app, we need a test runner (Jest provided by create-react-app) and testing utility (Enzyme) that allows us to shallow render a specific React component.

Since `Jest` comes with create-react-app, so we only need to install `Enzyme` by running the following NPM command.

```
// for React v16.2.0, refers to enzyme documentation for the latest package
npm i enzyme react-test-renderer enzyme-adapter-react-16
```

To set up a React project for testing, we need to first create a test file such as `NavigationItems.test.js` and put it in the same folder as the component. Then configure Enzyme to use the adapter you want it to use.

```js
import React from 'react';

import { configure, shallow } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

// config an Adapter
configure({ adapter: new Adapter() });
```

#### General Test Structure

```js
// use 'describe' to group together similar tests
describe('<NavigationItems />', () => {
  // use 'it' to test a single attribute of a target
  it('shows the correct text', () => {
    // use 'expect' to make an assertion about a target
    expect(component).toContain('React simple starter');
  });
});
```

#### Testing functional component using Jest

```js
describe('<NavigationItems />', () => {
  let wrapper;

  // shallow render the component before testing
  beforeEach(() => {
    wrapper = shallow(<NavigationItems />);
  });

  it('should render two NavigationItem elements if not authenticated', () => {
    expect(wrapper.find(NavigationItem)).toHaveLength(2);
  });

  it('should render three NavigationItem elements if authenticated', () => {
    wrapper.setProps({ isAuthenticated: true });
    expect(wrapper.find(NavigationItem)).toHaveLength(3);
  });

  it('should contain Logout <NavigationItem /> if authenticated', () => {
    wrapper.setProps({ isAuthenticated: true });
    expect(wrapper.contains(<NavigationItem link="/logout">Logout</NavigationItem>)).toEqual(true);
  });
});
```

#### Testing class component using Jest

To test class component, first you need to export the class component so you can use it in your test file. We usually test how the props would affect the class component instead of worrying about the Redux store part.

```js
export class BurgerBuilder extends Component { ...code }
```

```js
describe('<BurgerBuilder />', () => {
  let wrapper;

  // this.props.onInitIngredients() is called in componentDidMount(), so we need to define it in the component
  beforeEach(() => {
    wrapper = shallow(<BurgerBuilder onInitIngredients={() => {}} />);
  });

  it('should render BuildControls when receiving ingredients', () => {
    wrapper.setProps({
      ingredients: {
        salad: 0
      }
    })

    expect(wrapper.find(BuildControls)).toHaveLength(1);
  });
});
```

#### Testing reducers

```js
import reducer from './auth';
import * as actionTypes from '../actions/actionTypes';

describe('auth reducer', () => {
  it('should return the initial state', () => {
    expect(reducer(undefined, {})).toEqual({
      token: null,
      userId: null,
      error: null,
      loading: false,
      authRedirectPath: '/'
    })
  });

  it('should store the token upon login', () => {
    expect(reducer({
      token: null,
      userId: null,
      error: null,
      loading: false,
      authRedirectPath: '/'
    }, {
      type: actionTypes.AUTH_SUCCESS,
      idToken: 'token id',
      userId: 'user id'
    })).toEqual({
      token: 'token id',
      userId: 'user id',
      error: null,
      loading: false,
      authRedirectPath: '/'
    })
  });
});
```
