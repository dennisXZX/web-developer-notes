## Vue Router

#### Set up routes

create a router.js file which hosts all the routes.

``` 
import User from './components/user/User.vue';
import Home from './components/Home.vue';

export const routes = [
  { path: '', component: Home },
  { path: '/user', component: User },
];
```

Config the routes in main.js.

```js
import Vue from 'vue'
import VueRouter from 'vue-router'; // import vue-router
import { routes } from './routes'; // import routes
import App from './App.vue'

Vue.use(VueRouter); // use Vue Router plugin

// create a Vue router instance
const router = new VueRouter({
  routes,
  // if we config the server to returns 'index.html' on every request, we can get rid of the '#' in the url
  mode: 'history'
});

new Vue({
  el: '#app',
  router,  // register the router
  render: h => h(App)
})
```

Now you can use `<router-link>` to navigate between routes.

```html
<p>
  <!-- use router-link component for navigation. -->
  <!-- specify the link by passing the `to` prop. -->
  <!-- `<router-link>` will be rendered as an `<a>` tag by default -->
  <router-link to="/foo">Go to Foo</router-link>
  <router-link to="/bar">Go to Bar</router-link>
</p>
```

#### Route guard

- Per-Route Guard

```js
const routes = [
  { path: '/', component: WelcomePage },
  {
    path: '/dashboard',
    component: DashboardPage,
    beforeEnter(to, from, next) {
      // if the user is logged in then continue the route, else redirect the user
      // the JSON Web Token is stored in the state
      if (store.state.idToken) {
        next()
      } else {
        next('/signin')
      }
    }
  }
]
```

#### Navigate from code

You can push a route to the route stack by leveraging the `$router` object.

```js
this.$router.push({ path: '/' })
```

#### Hash vs History

The default behavior of the browser is on each request the whole URL is sent to the server. However, in a single page application, we want to handle the routes in the application instead of by the server.

When we use the `hash` mode, `example.com/#/user`, the part before # is sent to the server, the part after # is handed over to the running Javascript application.

We can config our server to return 'index.html' in all cases if we want to get rid of the # sign in the URL.
