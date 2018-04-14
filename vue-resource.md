#### vue-resource

```
// config vue-resource in main.js
import VueResource from 'vue-resource';

Vue.use(VueResource);
```

```js
// after config, we can use it in component as 'this.$http'
getUsers() {
  this.$http.get('http://jsonplaceholder.typicode.com/posts')
    .then(response => {
      this.posts = response.body
    }, error => {
      // error callback
    });
}
```
