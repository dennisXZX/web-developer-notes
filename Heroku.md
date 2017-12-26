# Heroku

### Heroku deployment with the app being in a subfolder

Sometimes your package.json is not in your project root directory, in such a case, Heroku would fail to build your app as its buildpack cannot handle your app due to detection failure. One way to address this issue is to push a subdirectory of your git repository to Heroku during deployment.

```js
git subtree push --prefix appFolderName heroku master
```

### Heroku deployment preparation

1. Dynamic port binding - Heroku tells us which port our app should use, so we need to make sure we listen to that port.

```js
const PORT = process.env.PORT || 5000;
// listen to the specific port in express
app.listen(PORT);
```

2. Specify Node.js environment - Specify which version of Node.js you will be using.

Specify node version and npm version in package.json file (generated after running `npm init`).

```js
"engines": {
  "node": "8.1.1",
  "npm": "5.0.3"
}
```

3. Specify start script - Instruct Heroku what command to run to start our server.

Specify a start script in the package.json file.

```js
"scripts": {
  "start": "node index.js"
}
```

4. Create .gitignore file - We don't want to upload app dependencies to Heroku.

Create a `.gitignore` file in the server folder.

```js
// ignore node dependencies
node_modules
```

### Heroku deployment

After installing Heroku CLI, you can use `heroku login` to login Heroku, then use `heroku create` to create your Heroku app. The `heroku create` command should generate two links as follows.

```js
// you should see two links after running 'heroku create'
// the first link is the URL for your app
// the second link is your deployment target, to where you should push your code
https://fast-falls-56230.herokuapp.com/ | https://git.heroku.com/fast-falls-56230.git
```

Make sure https://git.heroku.com/fast-falls-56230.git is set as a remote repository by running `git remote -v`. Use `git remote add heroku https://git.heroku.com/fast-falls-56230.git` to add it as a deployment target if it's not there.

When you finish setting up the remote heroku repository, you can then use `git push heroku master` to push your code to its master branch.

Finally, use `heroku open` to launch your app.

If you run into any issues during deployment, always use `heroku logs` to see what happens under the hood.

### Set up environment variables

Head to Heroku project setting and you should be able to see a `Config Variables` section where you can put in `Config Vars`. Once you have them set up, you can refer to these vars in your code as follows.

```js
// production keys
module.exports = {
  googleClientID: process.env.GOOGLE_CLIENT_ID,
  googleClientSecret: process.env.GOOGLE_CLIENT_SECRET,
  mongoURL: process.env.MONGO_URL,
  cookieKey: process.env.COOKIE_KEY
};
```
