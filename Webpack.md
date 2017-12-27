## Webpack

#### Webpack workflow

1. Specify an entry point where Webpack should start (multiple entry points are possible)
2. Apply loaders on per file basis (file-type dependent transformation)
3. Apply plugin on the bundled file (global transformation)
4. Output a bundled file

#### webpack.config.js

Development workflow

```js
const path = require('path');
const autoprefixer = require('autoprefixer');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  // generate a source map
  devtool: 'cheap-module-eval-source-map',
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
    // lazy loading (AKA code splitting)
    chuckFilename: '[id].js',
    publicPath: ''
  },
  // Webpack would try to add these extensions to the file, so we can omit file extensions during import
  resolve: {
    extensions: ['.js', '.jsx']
  },
  module: {
    rules: [
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

```js
const path = require('path');
const autoprefixer = require('autoprefixer');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  // generate a source map
  devtool: 'cheap-module-eval-source-map',
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
    // lazy loading (AKA code splitting)
    chuckFilename: '[id].js',
    publicPath: ''
  },
  // Webpack would try to add these extensions to the file, so we can omit file extensions during import
  resolve: {
    extensions: ['.js', '.jsx']
  },
  module: {
    rules: [
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

#### Webpack flag

```js
// generated minified production code
webpack -p
// generated a source-map for the code
webpack -d
// watch file changes
webpack --watch
```
