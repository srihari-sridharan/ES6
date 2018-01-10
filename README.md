# ES6 Seed

## Setting Up ES6 Using Babel and webpack

### Transpiling with Babel

* Ensure that node.js and npm are installed.
* Babel.js is an awesome tool that lets you transpile your ES6 code into ES5 code that can then be run in current JavaScript environments. `npm install -g babel-cli`
* Run any file with ES6 code from your command line using: `babel-node <filename.js>`
* But transpiling every file manually isn’t efficient and, in most cases, isn’t the solution for managing large projects. :-)

### Setting Up an ES6 Boilerplate

Follow the steps outlined below.

#### Start a New Project

The commands below create a new directory and initializes a new project using `npm init`.

```
mkdir es6-boilerplate
cd es6-boilerplate
npm init –yes
```

#### Install webpack and webpack-dev-server

Webpack is a very flexible module bundler that takes modules with dependencies and generates static assets representing those modules. We will be using `webpack` to let babelloader transpile our ES6 code into traditional ES5 code and generate a bundled output file.

```
npm install –-save-dev webpack
```

Use `webpack-dev-server` to serve our app and transpile the code on the fly

```
npm install –-save-dev webpack-dev-server
```

To read more about `webpack` and `webpack-dev-server` in detail, you can visit https://webpack.js.org/concepts

#### Install Babel in the Project

Install the following packages:

`npm install --save-dev babel-loader babel-core babel-preset-es2015`

The next step is to configure babel to use ES2015 presets by adding a new file
.babelrc in the root directory of your project with the following JSON:

```JSON
{
    "presets": ["es2015"]
}
```

Your package.json file should more or less look like this:

```JSON
{
    "name": "ES6",
    "version": "1.0.0",
    "description": "ES6",
    "devDependencies": {
        "babel-core": "^6.24.1",
        "babel-loader": "^6.4.1",
        "babel-preset-es2015": "^6.24.1",
        "webpack": "^2.4.1",
        "webpack-dev-server": "^2.4.2"
    }
}
```

Create an `index.html` and `index.js` in the home directory.

#### Configuring Webpack

The next step is to set up webpack by creating a configuration file - `webpack.config.js` in the root directory of your project.

``` JavaScript
// webpack.config.js

module.exports = {
    entry: './index.js',
    output: {
        path: './dist',
        filename: 'bundle.js'
    }
};
```


#### Add Loaders

Loaders allow you to preprocess files as you load them. Loaders provide a powerful way to handle front-end build steps and can transform files from a different language, like CoffeeScript to JavaScript or inline images as data URLs. For example, babel-loader uses Babel to load ES2015 files.

So now, modify webpack.config.js to process all .js files using babel-loader:

```JavaScript
module.exports = {
    entry: './index.js',
    output: {
        path: __dirname + '/dist',
        publicPath: '/dist/',
        filename: 'bundle.js'
    },
    module: {
        rules: [{
            test: /\.js$/,
            exclude: /node_modules/,
            use: 'babel-loader'
        }]
    }
};
```

The above configuration implies the following:
1. `./index.js` is the entry point of the application.
2. Output will be generated in `./dist/bundle.js`.
3. We are processing every `.js` using the `babel-loader`, excluding
`node_modules` to avoid external libraries to go through Babel,
slowing down compilation.

### Adding Your Generated bundle.js script to your index.html

Now we can include the bundle.js script into our html file to run the code.
```HTML
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>ES6</title>
</head>

<body>
    <h1>ES6</h1>
    <p>Check console for details</p>
    <div id="main"></div>
    <script src="dist/bundle.js"></script>
</body>

</html>
```

To compile your .js file, you can run the following command in your terminal

`webpack`

Or, you can add the below script command to your `package.json` and run it.

```JSON
"scripts": {
    "build": "webpack -d --progress --colors"
}
```

`npm run build`

You can also use following additional flags with webpack:
* `webpack` for building once for development.
* `webpack -p` for building once for production (minification).
* `webpack -w` for continuous incremental build in development (fast!).
* `webpack -d` to include source maps.

> Pro Tip : We can achieve prettier output using `webpack -- progress --colors` that add a progress bar and colors in the webpack output in the terminal.

#### Setting Up a Development Server

To start dev server:

`webpack-dev-server --hot --inline`

Update your package.json with scripts.

```JSON
"scripts": {
    "start": "webpack-dev-server --hot --inline",
    "watch": "webpack -w -d --progress --colors",
    "build": "webpack -p --progress --colors"
}
```

You can launch the server using `npm run server`. This hosts the sever in http://localhost:8080.

#### Upgrading to babel-preset-env

* Please refer to link http://babeljs.io/env#upgrading-to-babel-preset-env.
* Install `npm install babel-preset-env --save-dev`
* Basic `.babelrc` change

```JSON
{
   "presets": ["env"]
}
```

* `.babelrc` against a specific chrome version

```JSON
{
  "presets": [
    ["env", {
      "targets": {
        "chrome": "60"
      }
    }]
  ]
}
```

* `.babelrc` against current node version

```JSON
{
  "presets": [
    ["env", {
      "targets": {
        "node": "current"
      }
    }]
  ]
}
```