# Heroku

#### Deploy on Heroku

1. Dynamic port binding - Heroku tells us which port our app should use, so we need to make sure we listen to that port.

```
const PORT = process.env.PORT || 5000;
// listen to the specific port in express
app.listen(PORT);
```

2. Specify Node.js environment - Specify which version of Node.js you will be using.

Specify node version and npm version in package.json file (generated after running `npm init`).

```
"engines": {
  "node": "8.1.1",
  "npm": "5.0.3"
}
```

3. Specify start script - Instruct Heroku what command to run to start our server.

Specify a start script in the package.json file .
```
"scripts": {
  "start": "node index.js"
}
```

4. Create .gitignore file - We don't want to upload app dependencies to Heroku.

Create a `.gitignore` file in the server folder.

```
// ignore node dependencies
node_modules
```
