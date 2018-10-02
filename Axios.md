## Axios

#### Axios.all()

Axios.all() can perform multiple concurrent requests.

```js
axios.all([getUserAccount(), getUserPermissions()]).then(
  axios.spread(function(account, permissions) {
    // Both requests are now complete
  })
);
```

#### Instance

Sometimes you need a base URL that is different from the global URL, or you would like to customize some config for a specific request. In such a situation, Axios instance really comes in handy.

```js
import axios from 'axios';

// Set config defaults when creating the instance
const instance = axios.create({
  baseURL: 'https://react-burger-app-29cd2.firebaseio.com/'
});

// Alter defaults after instance has been created
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;

export default instance;
```

#### Interceptors

You can intercept requests or responses before they are handled by `then` or `catch`. Interceptor acts as a middleware, so you must return the config

```js
// Add a request interceptor
const reqInterceptor = axios.interceptors.request.use(
  function(config) {
    // Do something before request is sent
    return config;
  },
  function(error) {
    // Do something with request error
    return Promise.reject(error);
  }
);

// Add a response interceptor
const resInterceptor = axios.interceptors.response.use(
  function(response) {
    // Do something with response data
    return response;
  },
  function(error) {
    // Do something with response error
    return Promise.reject(error);
  }
);

// remove both the request and response interceptors
axios.interceptors.request.eject(reqInterceptor);
axios.interceptors.response.eject(resInterceptor);
```

#### Global Axios defaults

You can set up some global config on Axios. One of the most important usages is to set up a base URL for all your requests.

```js
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
axios.defaults.headers.get['Accepts'] = 'application/json';
```

#### Example Axios httpService

```js
// File: httpService.js import axios from "axios";

axios.interceptors.response.use(null, error => {
  const expectedError =
    error.response &&
    error.response.status >= 400 &&
    error.response.status < 500;

  if (!expectedError) {
    console.log('An unexpected error occurrred.');
  }
  return Promise.reject(error);
});

function setJwt(jwt) {
  axios.defaults.headers.common['x-auth-token'] = jwt;
}

export default {
  get: axios.get,
  post: axios.post,
  put: axios.put,
  delete: axios.delete,
  setJwt
};

// File: authService.js
import http from './httpService';
import { apiUrl } from '../config.json';

const apiEndpoint = apiUrl + '/auth';
const tokenKey = 'token';

http.setJwt(getJwt());

export async function login(email, password) {
  const { data: jwt } = await http.post(apiEndpoint, { email, password });
  localStorage.setItem(tokenKey, jwt);
}
```
