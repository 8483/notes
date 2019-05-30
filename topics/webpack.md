Webpack is a bundler, which combines many javascript files into a single bundle file.

It also uglifies (shortens identifier names) and minifies (gets rid of whitespace) the code.

```bash
sudo npm install webpack webpack-cli --save-dev
```

## Simple Default Example

**Webpack needs an `src` directory with the entry `index.js` file.**  

Without this, we get `ERROR in Entry module not found: Error: Can’t resolve ‘./src’ in 'folder'`. The entry point can be changed in a `webpack.config.js` file.

We need to add a webpack scripts in `package.json`, which gets all the dependencies from `./src/index.js`, and bundles all the code into a `./dist/main.js` file.

```json
{
    "scripts": {
        "build": "webpack"
    },
    "devDependencies": {
        "webpack": "4.x.x"
    }
}
```
We then simply run
```bash
npm run build
```

These can also be used in `package.json`, where production minifies, and development doesn't.
```json
"scripts": {
    "dev": "webpack --mode development",
    "build": "webpack --mode production"
}
```

## Configuration

Instead of a `package.json` script, we can use a detailed `webpack.config.js` configuration file, which can be generated with the command below and by answering questions.  

```bash
webpack init
```
Example :
1. Will your application have multiple bundles? **No**
2. Which module will be the first to enter the application? [example: './src/index.js'] **./src/index.js**
3. Which folder will your generated bundles be in? [default: dist] **Enter**
4. Are you going to use this in production? **No**
5. Will you be using ES2015 (ES6)? **Yes**
6. Will you use one of the below CSS solutions? **No**
7. Name your `webpack.[name].js`? [default: 'config'] **Enter**

This will install:
- webpack-cli
- uglifyjs-webpack-plugin
- babel-core
- babel-loder
- babel-preset-env
- webpack

It will also generate the `webpack.config.js` file:
```javascript
const webpack = require("webpack");
const path = require("path");

const UglifyjsJSPlugin = require("uglifyjs-webpack-plugin");

module.exports = {
    entry: './src/index.js',
    output: {
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, dist)
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader',
                options: {
                    presets: ['env'] 
                }
            }
        ]
    },
    plugins: [new UglifyJSPlugin()]
};
```
We can add this in `package.json` watch for changes and rebuild:
```json
{
    "scripts": {
        "build": "webpack -w"
    }
}
```