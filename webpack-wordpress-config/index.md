---
title: Using Webpack to compile Javascript in an Wordpress Plugin
cover: webpack-logo.png
tags: webpack, wordpress, development, node
date: 2019-10-28
---

## Installation

So, the future of WordPress is Javascript!

Just look at Guttemberg, the new editor for WordPress. Is completelly written using React.

So if you want to work on WordPress, you need to use JavaScript... And if you want to work in JavaScript, you have to bundle your assets.

... But is Asset bundling you ask?

Is the process of compiling all of your javascript, css and even images in just one javascript that can be included in your HTML (or in our case, enqueued in with `add_action('wp_enqueue_script', '')` ).

Some of the advantages of bundling files are:

- You'll end up with just one assets file
- You cand _develop_ your JavaScript using modern techniques like `importing` and _arrow functions_
- You'll catch errors on build time and not at the deployment
- You wil have to manage smaller files

If you want to know more about `webpack` and asset bundling, head over to the [documentation page](https://webpack.js.org/concepts/) and have a quick read.

## Starting your project.

I'm going to assume you already know how to create WordPress plugins. So I'm not going to go over how to do that. Instead I'm going to show you how to start with `webpack`.

So first create a `package.json` file. And then add `webpack` to it.

```bash
npm init -y
npm install --save-dev webpack webpack-cli
preset-env
```

Also, lets add `babel` so we can write our javascript using ES6 and have it compile to a format more compatible with older browsers:

```bash
npm install --save-dev babel-loader @babel/core @babel/cli @babel/preset-env
```

If you executed those commands correctly, you should have a file in your directory called `package.json` similar to the following:

```json
{
  "name": "test-webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/cli": "^7.7.0",
    "@babel/core": "^7.7.2",
    "@babel/preset-env": "^7.7.1",
    "babel-loader": "^8.0.6",
    "webpack": "^4.41.2",
    "webpack-cli": "^3.3.10"
  }
```

## Configure

Until now you have a list of packages to install, but still don't have nothing to do.

To make our life easier, I'm going to add the _start_ script, so I can execute `npm start` and have webpack bundle our scripts.

Lets just edit the _scritps_ section replacing the _test_ script with the following:

```json{7}
{
  "name": "test-webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "webpack"
  }
  // ...
}
```

And I'll test that

![Run webpack and get an error](webpack-start-error.png)

ERORR!!! You didn't tought it was going to be easy, right?

## WebPack config

We have our packages installed, and our script configure. Now we have to instruct webpack what to bundle, how to bundle it and where to put it.

Webpack reaquires a `webpacka.config.js` file in order to know all that.

```js
// webpack.config.js
const path = require("path")

module.exports = {
  entry: "./src/js/frontend.js",
  output: {
    filename: "[name].js",
    path: path.resolve(__dirname, "js"),
  },
}
```

Also, lets create the `src/js/frontend.js` file that we just told `webpack` to look for:

```js
// src/js/frontend.js
const myFunction = () => {
  console.log("frontend.js starting file")
}
myFunction()
```

![Execute Webpack with success](webpack-start-success.png)

If you look into `js/main.js` you'll see a javascript file with a bunch of jiberish and at the end the `frontend.js starting file` content at the end

![Frontend compiled with webpack](frontend-compiled.png)

Now we have webpack working and correctly

... But that's not all.

## Specify names

Maybe you've noticed, but the output file was named `main.js` instead of `frontend.js`. That's because we didn't used names on the entry points.

So lets do that. Lest use the name `frontend` as the entry poing. Lest edit `webpack.config.js` and change make some changes:

```js{5-7}
// webpack.config.js
const path = require("path")

module.exports = {
  entry: {
    frontend: "./src/js/frontend.js",
  },
  output: {
    filename: "[name].js",
    path: path.resolve(__dirname, "js"),
  },
}
```

and execute `npm start` again.

You should have a `js/frontend.js` file now.

## Adding babel

At the begeining we installed webpack with babel, but didn't do anything with it. That's because we haven't seen anything about loaders yet.

Loaders are modules that `convert` the input into an special output.

The `babel-loader` converts all the `js` files into backards compatible ones before saving the file into the destination folder.

```js{15,20}
// webpack.config.js
const path = require("path")

module.exports = {
  entry: {
    frontend: "./src/js/frontend.js",
  },
  output: {
    filename: "[name].js",
    path: path.resolve(__dirname, "js"),
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "babel-loader",
      },
    ],
  },
}
```

What I'm doing here is:

- Loking for `js` files with the regular expresions `/\.js$/`
- Excluding the `node_modles` folder
- pasing those files trough the `babel-loader`

Now the output file, which is `js/frontend.js` will be compiled from ES5 to an version that is more compatible.

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
