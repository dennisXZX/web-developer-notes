## Vuex

#### Create a central store

Normally we put all our state in a separate folder called store.

```js
// store.js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export const store = new Vuex.Store({
  state: {
    counter: 0
  },
  getters: {
    doubleCounter(state) {
      return state.counter * 2;
    },
    stringCounter(state) {
      return state.counter + ' Clicks';
    }
  },
  mutations: {
    increment(state, payload) {
      state.counter += payload;
    },
    decrement(state, payload) {
      state.counter -= payload;
    }
  },
  actions: {
    // synchronous actions
    increment(context, payload) {
      context.commit('increment', payload);
    },
    // asynchronous actions
    asyncIncrement(context, payload) {
      setTimeout(() => {
        context.commit('increment', payload);
      }, 1000);
    }
  }  
});
```

Register the store in the component

```js
// import the store
import { store } from './store/store';

new Vue({
  el: '#app',
  store, // register the store
  render: h => h(App)
});
```

- Access the state using `this.$store.state.counter`
- Access the getter using `this.$store.getter.doubleCounter`
- Commit the mutation using `this.$store.commit('increment')`

#### Structure your store

We can separate our state into multiple modules.

```
-- store
   |-- modules
   |   |-- counter.js
   |   |-- value.js
   |-- store.js   
```

For example, the counter.js can look something like this. 

```js
const state = {
  counter: 0
};

const getters = {
  doubleCounter(state) {
    return state.counter * 2;
  }
};

const mutations = {
  increment(state, payload) {
    state.counter += payload;
  }
};

const actions = {
  asyncIncrement(context, payload) {
    setTimeout(() => {
      context.commit('increment', payload);
    }, 1000);
  }
};

export default {
  state,
  getters,
  mutations,
  actions
}
```

Then in the store.js, we import all the modules and place them in the modules object.

```js
import counter from './modules/counter';

Vue.use(Vuex);

export const store = new Vuex.Store({
  modules: {
    counter
  }
});
```

If you want to further break down your file, you can create separate `actions.js`, `getters.js` and `mutations.js` for each module.

#### mapGetters

For example, we have two getters and we want to map them to a computered properties.

```js
getters: {
  doubleCounter(state) {
    return state.counter * 2;
  },
  stringCounter(state) {
    return state.counter + ' Clicks';
  }
}
```

We can use the `mapGetters` function provided by vuex. But we need to install `babel-preset-stage-2` in order to use the spread operator.

```js
<script>
  import { mapGetters } from 'vuex';

  export default {
    computed: {
      ...mapGetters({
        counter: 'doubleCounter',
        clicks: 'stringCounter'
      }),
      otherComputedProp() {
        // code...
      }
    }
  };
</script>
```

#### mapMutations

```js
<script>
  import { mapMutations } from 'vuex';

  export default {
    methods: {
      ...mapMutations({
        increment: 'increment',
        decrement: 'decrement'
      }),
      otherMethod() {
        // ...code
      }
    }
  };
</script>
```

#### Actions for async mutations

Mutations can only execute synchronous code, if you need to do some asynchronous events before mutations, then you need to rely on actions.

```js
<script>
  import { mapActions } from 'vuex';

  export default {
    methods: {
      // map actions to the component
      // under the hood, actually it's doing this.$store.dispatch('asyncIncrement', payload)
      // if you need to pass multiple values, you can make payload as an object
      ...mapActions({
        asyncIncrement: 'asyncIncrement',
        asyncDecrement: 'asyncDecrement'
      }),
      otherMethod() {
        // ...code
      }
    }
  };
</script>
```

Then you can dispatch the action in your template.

```html
<!-- if you need to pass multiple values, use @click="decrement({ by: 50, duration: 1000 })" -->
<button class="btn btn-primary" @click="decrement(100)">Decrement</button>
```
