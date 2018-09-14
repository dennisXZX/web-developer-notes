## NPM

#### NPX

NPX is a tool for executing Node packages introduced after `npm@5.2.0`.

You can run a locally installed package using `npx package_name`.

In addition, you can use it to run one-off command `npx create-react-app my-app` so you don't need to pollute global installs.

#### Publish and update a package

Publish a package

- `npm login` to login your NPM account
- `npm publish` to publish the package

Update a package

- Make changes to your package
- Change the version number in `package.json` by running `npm version major|minor|patch`
- `npm publish` to publish the updated package

#### Update outdated packages

`npm outdated` to check outdated local packages. You can add `-g` flag to check global outdated packages.

Run `npm update` to update the outdated packages, however, this will only update minor and patch versions. You would need to run `npm i packageName` to install the latest package.

#### View registry info of a package

- `npm view packageName dependencies` to view all the dependencies of a package

- `npm view packageName versions` to view all the released versions of a package

#### NPM initiation

You can create a `Package.json` with default settings by running `npm init --yes`.

#### Semantic versioning

NPM packages follow a semantic versioning pattern, such as, `^4.13.6`, which represents `Major.Minor.Patch`.

- Patch version is for bug fix.
- Minor version is for adding new features that don't break existing APIs
- Major version is for adding new features that potentially might break the existing APIs

`^4.13.6`: The caret (^) indicates that we would use the latest package as long as the major version stay the same

`~4.13.6`: The tilde (~) indicates that we would use the latest package as long as the major and minor version stay the same

`4.13.6`: NPM would install exactly the same version

#### Install a package with a specific version

```bash
// use the @ operator to specify a package version
(sudo) npm i -g npm@5.5.1
```

#### Lock the package version

You can use the `--save-exact` flag to lock the package version.

```bash
npm i angularfire2@4.0.0-rc.2 --save --save-exact
```

#### Install only dev dependencies

You can just install dev dependencies by running the following command.

```bash
npm install --only=dev
```

#### Proxy

We can set up a proxy to forward a request to other API. The following code snippet forwards any request to `/auth/google` to `http://localhost:5000`. This proxy can act as a bridge between front-end and back-end server.

```
"proxy": {
  "/auth/google": {
    "target": "http://localhost:5000"
  }
}
```

#### NPM script

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
  
  // use concurrently to run backend and frontend dev servers at the same time
  "dev": "concurrently \"npm run server\" \"npm run client\""
  
  // run Webpack with a specified config file and in watch mode
  "dev:build:server": "webpack --config webpack.server.js --watch"
  
  // everytime the files in build folder change, nodemon execute 'node build/bundle.js'
  "dev:server": "nodemon --watch build --exec \"node build/bundle.js\"",
}
```
