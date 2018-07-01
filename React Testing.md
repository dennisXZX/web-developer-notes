## React Testing

To test in the frontend, we need a suite of toolkit.

- Test framework, which acts as a scaffolding for the testing. (Jasimine, Jest, Mocha)
- Assertion library, which verifies that things are correct. (Jasmine, Jest, Chai)
- Test runner, which runs your tests in browser (DOM), headless browser (Puppeteer) or JsDOM. (Jasmine, Jest, Mocha, Karma)
- Spy, Stub and Mock, which helps you test your functions easily (Jasmine, Jest, Sinon)
  - Spy provides info about your function
  - Stub replaces a function with code that returns a specified result
  - Mock fakes a function
- Code coverage, which gives you coverage report on your test. (Jest, Istanbul)

__Unit Test__

Test synchronous function:

```js
// import that function that need to be tested
const googleSearch = require('./script');

// mock the database
dbMock = [
  'abc.com',
  'def.com'
]

// group relevant test cases
describe('googleSearch function', () => {
  it('searches "dog"', () => {
    // user assertion library to verify the results
    expect(googleSearch('dog', dbMock)).toEqual([]);
  })
  
  it('work with undefined', () => {
    expect(googleSearch(undefined, dbMock)).toEqual([]);
  })  
})
```

Test asynchronous function:

```js
const fetch = require('node-fetch');

it('calls swapi to get people object', (done) => {
  // expect one assertion happens
  expect.assertions(1);
  
  swapi.getPeople(fetch).then(data => {
    expect(data.count).toEqual(87);
    // call done() to signal that the test is finished
    // this is necessary in testing asynchronous function
    done();
  })
})

it('getPeople returns count and results', (done) => {
  // mock the fetch function
  const mockFetch = jest.fn()
    .mockReturnValue(Promise.resolve({
      json: () => Promise.resolve({
        count: 89,
        results: [0, 1, 2, 3, 4, 5, 6]
      })
    }))
  
  expect.assertions(4);
  
  swapi.getPeople(fetch).then(data => {
    expect(mockFetch.mock.calls.length).toBe(1);
    expect(mockFetch).toBeCalledWith('https://swapi.co/api/people');
    expect(data.count).toEqual(87);
    expect(data.results.length).toBeGreaterThan(5);
  })
}) 
```

#### Set up testing in React

In order to test a React app, we need a test runner (Jest provided by create-react-app) and testing utility (Enzyme) that allows us to shallow render a specific React component.

Since `Jest` comes with create-react-app, so we only need to install `Enzyme` by running the following NPM command.

```
npm i --save-dev enzyme enzyme-adapter-react-16
```

To set up a React project for testing, we need to first create a test file such as `NavigationItems.test.js` and put it in the same folder as the named-after component. Then create a Enzyme config file `setupTests.js` in the `src` folder to use the adapter you want it to use.

```js
/* setupTests.js */

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
// shallow allows you to render a component but not its nested components
// so you can focus on just testing one component at a time
import { shallow } from 'enzyme';
import React from 'react';

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
    
    // simulate a click on the button
    wrapper.find('[id="counter"]').simulate('click');
    
    // verify the state after the click event
    expect(wrapper.state()).toEqual({ count: 2 });
    
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

#### Snapshot test

```js
it('expect to render Card component', () => {
  // generate a snapshot of the Card component
  expect(shallow(<Card />)).toMatchSnapshot();
})
```

#### Generate code coverage

If you run Jest via npm test, you can still use the command line arguments by inserting a `--` between npm test and the Jest arguments.

```
npm test -- --coverage
```
