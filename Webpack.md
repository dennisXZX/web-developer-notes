## Webpack

Webpack is a bundler that can handle ESM, CommonJS and AMD modules with tree-shaking.

#### NPM tips

- All executables are placed inside `node_modues/.bin`.

- NPM scripts can let you use all executables in `.bin` directly as if they are all in scope. 

- You can compose scripts and pass parameter to script by `--`.

```
scripts: {
    "webpack": "webpack",
    "dev": "npm run webpack -- --mode development"
}
```

#### Webpack workflow

1. Specify an entry point where Webpack should start (multiple entry points are possible)
2. Apply loaders on per file basis (file-type dependent transformation)
3. Apply plugin on the bundled files (global transformation)
4. Output bundled files to the specified path

#### Loaders

1. Make sure they are in correct order. (right to left)

2. Loaders are functions that take the source of a resource file as the parameter and return the new source.

3. Loaders can be chained. They are applied in a pipeline to the resource. The final loader is expected to return JavaScript; each other loader can return source in arbitrary format, which is passed to the next loader.

For example, if you have `somefile.css` and you are passing it through loaderOne, loaderTwo, loaderThree is behaves like a regular chained function.

```js
{
    test: /\.css$/,
    loaders: ['loaderOne', 'loaderTwo', 'loaderThree']
}
```

means exactlly the same as...

```js
loaderOne(loaderTwo(loaderThree(somefile.css)))
```

#### webpack.config.js

Development workflow

```js
const path = require('path');
const autoprefixer = require('autoprefixer');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  // no matter where you run Webpack, it always run from the root directory
  context: __dirname,
  // specify how Webpack should resolve files
  resolve: {
    modules: [
      path.resolve('./lib'),
      path.resolve('./node_modules')
    ]
  },
  // generate a source map
  devtool: 'cheap-module-eval-source-map',
  // use babel-polyfill so the compiled code can be used in all browsers
  entry: ['babel-polyfill', './src/index.js'],
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
    // lazy loading (AKA code splitting)
    chuckFilename: '[id].js',
    publicPath: ''
  },
  // server your static files using an HTTP server
  devServer: {
    hot: true,
    publicPath: '/public/',
    historyApiFallback: true
  },  
  // Webpack would try to add these extensions to the file, so we can omit file extensions during import
  resolve: {
    extensions: ['.js', '.jsx']
  },
  // how Webpack report should look like
  stats: {
    colors: true,
    reasons: true,
    chunks: false
  },  
  module: {
    rules: [
      // run eslint before Babel
      {
        enforce: 'pre', // ensure the linting happens before Babel
        test: /\.jsx?$/,
        loader: 'eslint-loader',
        exclude: /node_modules/
      },    
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/
      },
      {
        // css-loader tells Webpack how to deal with CSS file
        // style-loader inject CSS code to the HTML header session
        // Keep in mind that Webpack processes loaders from right to left
        test: /\.css$/,
        exclude: /node_modules/,
        use: [
          { 
            loader: 'style-loader'
          },
          { 
            loader: 'css-loader', 
            // use CSS module feature, so a unique ID will be generated for each style
            options: {
              // tell css-loader there is 1 other loader run prior to it
              importLoaders: 1,
              modules: true,
              localIdentName: '[name]__[local]__[hash:base64:5]'
            }
          },
          // autoprefixer adds vendor prefixes to CSS rules automatically
          {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss',
              plugins: () => [
                autoprefixer({
                  browsers: [
                    "> 1%",
                    "last 2 versions"
                  ]
                })
              ]
            }
          }
        ]
      },
      {
        // url-loader check an image file, if it's within the limit it would place them in the code
        // if it exceeds the limit it would just copy the file into a specified folder
        test: /\.(png}jpe?g|gif)$/,
        loader: 'url-loader?limit=8000&name=images/[name].[ext]'
      }
    ]
  },
  plugins: [
    // inject the bundle.js file into the HTML template
    new HtmlWebpackPlugin({
      template: __dirname + '/src/index.html',
      filename: 'index.html',
      inject: 'body'
    })
  ]
};
```

#### webpack.prod.config.js

Production workflow

In the `package.json`, use `"build": "rimraf dist && webpack --config webpack.prod.config.js --progress --profile --color"` for production build. The `rimraf dist` command deletes the dist folder before running the build, so you have a clean slate to work with.
