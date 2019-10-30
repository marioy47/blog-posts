---
title: Using Webpack to compile Javascript in an Wordpress Plugin
permalink: webpack-compile-wordpress-plugin
---

## InstallationInstallationInstallationInstallation

```bash
npm init -y
npm install --save-dev webpack webpack-cli
npm install --save-dev babel-loader @babel/core @babel/cli @babel/preset-env
```

## Configure

```json
  "main": "index.js", // Remove this line
  "private": true,
  "scripts": {
    "webpack": "webpack"
  },
```

![Run webpack and get an error]()

## WebPack config
Create a hello file
```javascript
const path = require('path');

module.exports = {
  entry: './src/js/frontend.js',
  output: {
    filename: '[name].js',
    path: path.resolve(__dirname, 'js')
  }
};

```
[Image with both codes and the webpack output]()

## Specify names

```javascript
const path = require('path');

module.exports = {
  entry: {
    frontend: './src/js/frontend.js'
  },
  output: {
    filename: '[name].js',
    path: path.resolve(__dirname, 'js')
  }
};
```

## Adding babel
```javascript
const path = require('path');

module.exports = {
  entry: {
    frontend: './src/js/frontend.js'
  },
  output: {
    filename: '[name].js',
    path: path.resolve(__dirname, 'js')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader'
      }
    ]
  }
};
```

## Develop vs Build

```json
  "scripts": {
    "watch": "webpack --mode=development --watch",
    "build": "webpack --mode=production"
  },
```


## SASS
```bash
npm install sass-loader node-sass extract-loader file-loader --save-dev
```
