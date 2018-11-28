## reselect

#### The click moment of reselect

```js
/* selector file */

import { createSelector } from 'reselect'

// selectors
const getBar = (state) => state.foo.bar
const getKey = (state, ownProps) => ownProps.productOfferKey

// create a new instance of your selector function
// which is an anonymous function that returns a selector function
export const makeGetBarState = () => createSelector(
  [ getBar, getKey ],
  (bar, key) => {
    if (bar) {
      return bar + key;
    }
    
    return undefined;
  }
)
```

```js
/* React component file */

import React from 'react'
import { connect } from 'react-redux'
import { getBarState } from '../selectors'

const mapStateToProps = (state, ownProps) => {
  // get an instance of the selector function
  const getBarState = makeGetBarState();

  // return an api key that matches the product offer in the URL
  const apiKey = productOfferURLtoKey(ownProps.match.params.apiKey);

  return {
    // call the selector function, passing in state and an object
    // getBarState() will call getBar and getKey, with state and the object as arguments
    bar: getBarState(state, { apiKey })
  }
}

class Thing extends React.Component {
  ...
}

export default connect(mapStateToProps)(ReactComponent)
```
