## NPM

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
