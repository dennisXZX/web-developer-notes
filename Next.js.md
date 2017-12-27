## Next.js

Next.js is a framework for server-rendered React applications. It also sets up routing for you based on your file system. Every .js file in the `pages` folder becomes a route that gets automatically processed and rendered.

Once you have Next.js installed, you can create a `pages` folder and place an `index.js` in it, which will then be automatically mapped to `http://localhost:3000`. Keep in mind that Next.js does not change the way you build a React app. Your app should still be composed of React components.

#### Navigate between routes

```js
import React from 'react';
import Link from 'next/link';
import Router from 'next/router';

const indexPage = () => (
  <div>
    <h1>Main page</h1>
    <p>go to <Link href="/auth"><a>Auth</a></Link></p>
    <button onClick={() => Router.push('/auth')}>Go to Auth</button>
  </div>
);

export default indexPage;
```
