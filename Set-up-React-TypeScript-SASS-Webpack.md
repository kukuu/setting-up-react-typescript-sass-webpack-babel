# Setting up a React + TypeScript + SASS + Webpack and Babel

1. Initializing a blank project

Let's start by creating an empty folder and initializing a blank node project.

```
mkdir awesome-app

cd awesome-app

yarn init
```

Which will result in the following file structure:

```
.
└── package.json
```


2. Installing dependencies


Trnspiling code - There are three reasons, why we need to transpile our code:

a. Browsers don’t support TypeScript (at the moment), so we need to transpile it to JavaScript.

b. Not all browsers support modern JavaScript (especially IE). We need to transpile it into an “older version”, so that common browsers can interpret it correctly.


c. We need to transpile our SASS to CSS, because (you might guessed it already) nearly no browser supports SASS.

Install all the dependencies we need:

```

yarn add core-js react react-dom regenerator-runtime
yarn add -D  @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript @types/react @types/react-dom babel-loader css-loader node-sass sass-loader source-map-loader style-loader webpack webpack-cli typescript ts-loader

```


D. Description of packages:

```

react  // React framework

react-dom  // React's DOM framework
```

```
core-js  // Polyfills for a lot of ECMAScript methods
regenerator-runtime // Polyfill for runtime


```

```
/* Webpack */
webpack

webpack-cli

```


```
/* Babel core and presets to transpile TypeScript */

@babel/core 

@babel/preset-env 

@babel/preset-react 

@babel/preset-typescript

```



```/* Type Definitions for React */
@types/react 
@types/react-dom babel-loader
```

```
/* For transpiling SASS */

style-loader 
css-loader
node-sass
sass-loader

```

```
/* For better debugging */
source-map-loader
ts-loader
typescript

```


3. Setting up Webpack

Create our webpack.config.js file

```

module.exports = {
  mode: "development",
  watch: true,
  entry: "./src/index.tsx",
  output: {
    filename: "bundle.js",
    path: __dirname + "/dist"
  },
  resolve: {
    extensions: [".ts", ".tsx", ".js", ".json"]
  },
  devtool: "source-map",
  module: {
    rules: [
      { test: /\.scss$/, use: [ "style-loader", "css-loader", "sass-loader" ] },
      { test: /\.tsx?$/, loader: "babel-loader" },
      { test: /\.tsx?$/, loader: "ts-loader" },
      { enforce: "pre", test: /\.js$/, loader: "source-map-loader" }
    ]
  }
};

```

devtool: "source-map" will add source maps, which will make your developer life a lot easier, since they provide TypeScript sources to your browser devtools, so class names, interfaces and so on don’t get lost. Important: Remove this option and line 17 for a production build.




["style-loader", "css-loader", "sass"-loader"] is a loader chain. Which basically means that the output (return) from the right loader will be used as the input by the next loader and so on (right to left!). This loader chain will extract SASS from the SASS files, transpile it to CSS and finally to JavaScript.



babel-loader will transpile TypeScript to JavaScript (ES2015 in our case) based on the .babelrc file which we will configure in the next step.