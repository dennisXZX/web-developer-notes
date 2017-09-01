## NPM

### Proxy

We can set up a proxy to forward a request to other API. The following code snippet forwards any request to `/auth/google` to `http://localhost:5000`. This proxy can act as a bridge between front-end and back-end server.

```
"proxy": {
  "/auth/google": {
    "target": "http://localhost:5000"
  }
}
```

### NPM script

`--prefix` flag to specify the location where the script should be run.

We can use `concurrently` package to run multiple scripts at the same time.

```
"scripts": {
  // start up the backend server
  "start": "node index.js",
  // start up the backend dev server
  "server": "nodemon index.js",
  // start up the frontend dev server
  // running "npm run client" will actually run the command "npm run start" in the client folder
  "client": "npm run start --prefix client",
  // use concurrently to run backend and frontend server at the same time
  "dev": "concurrently \"npm run server\" \"npm run client\""
}
```
