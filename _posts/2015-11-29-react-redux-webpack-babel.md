---
layout: post
title:  "react + redux + webpack + babel + hot module reloading"
date:   2015-11-29 10:03:13
comments: true
---

Writing a web app with React and Redux (an implementation of flux architecture) is truly a joy... once it's set up. Setting it up, on the other hand, is not quite as enjoyable. Let's look at how to set up <a href="https://webpack.github.io/" target="_blank">webpack</a> and <a href="https://babeljs.io/" target="_blank">babel</a> to compile your React (and possibly ES6) code, as well as implement hot module reloading. Since all these libraries are new and rapidly changing, getting all of the the packages to play nicely together can be a headache. Here's how to do it (on November 29, 2015, at 3:13pm PT at least). 

#### Webpack

Webpack is a "module bundler". Make sense? Cool, let's move on.

Just kidding, of course! Webpack can be hard to wrap your head around at first. Basically, it allows you to write JavaScript for the browser in a module(ar) way, as you would on the server with Node. Node has built-in functionality for using `require` statements in your code, which imports code (modules, or files) into other modules (or files) that you can then use in that code. This allows you to write clear, modular code, and separate functionality into different files and directories. 

However, the browser does not support modules. So, you need a tool that traverses your code, performs the module imports under the hood, and then outputs a single, bundled JavaScript file, which is then consumed by the browser to run your application. Browserify and webpack are both viable options for performing this service, although webpack seems to play nicer with React and Redux. So let's use it.

#### Webpack plugins

Webpack comes with standard functionality, but requires helper libraries to perform tasks specific to your needs. For example, if you're writing a React + Redux app, you are likely using JSX (in your React components) and/or ES6 syntax (because many React and Redux docs are written in ES6). Unfortunately, browsers are unable to understand JSX or ES6. So, tough luck!

Again, just kidding! Since webpack is already traversing all of your code and dependencies, why not make it also compile any JSX and ES6 into JavaScript? Luckily, we can add this functionality with helper packages. Babel is a great compiler for both JSX and ES6. 

#### Hot module reloading

As I mentioned, webpack must traverse all your React code to compile and bundle it into a file that is usable by the browser. But what happens when you make a small change to one of your React components? Do you have to wait for webpack to re-bundle your entire application?

Not with hot module reloading! Hot module reloading (HMR for short) is a plugin that allows webpack to insert any changes "inline" without having to re-bundle the entire app. This saves a lot of time and also makes you feel like a superstar developer. Win win. 

#### Version numbers matter

Like, seriously matter. Like, will-give-you-a-headache-if-you're-not-careful matter. The reason for this is that a lot of these libraries (React, Redux, Babel) are relatively new and are rapidly changing. Sometimes, these changes are breaking and result in incompatibilities with the other helper libraries you're using. 

For example, Babel recently released v6.0. Unfortunately, v6.0 led to breaking changes with babel-plugin-react-transform, which allows for hot module reloading.

So... yeah. Version numbers matter. I will go through a current set up that works for me... right now. But my general advice is that if you find an example online that is working, just copy those version numbers into your `package.json` and run with it.

#### So what do I need to install?

Here's a list of what you need:

Babel: 

{% highlight javascript %}
"babel-core": "^5.8.3",
"babel-loader": "^5.3.2",
"babel-runtime": "^5.8.20"
{% endhighlight %}

HMR:

{% highlight javascript %}
"babel-plugin-react-transform": "^1.1.0",
"react-transform-hmr": "^1.0.1"
{% endhighlight %}

Webpack:

{% highlight javascript %}
"webpack": "^1.12.9",
"webpack-dev-middleware": "^1.4.0",
"webpack-hot-middleware": "^2.5.0"
{% endhighlight %}

The most important part of all this is that the HMR libraries are not yet ready for Babel v6.0, so you will need to use v5.x. There are developers working on upgrading the HMR packages, and it should happen soon. If you're interested, this would be a great open-source contribution to make. 

#### webpack.config.js

The `webpack.config.js` file is where you specify all of webpack's configurations for your specific project. Here's an example config: 

{% highlight javascript %}
var path = require('path')
var webpack = require('webpack')

module.exports = {
  devtool: 'inline-source-map',
  entry: [
    'webpack-hot-middleware/client',
    './client/client.js'
  ],
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js',
    publicPath: '/'
  },
  plugins: [
    new webpack.optimize.OccurenceOrderPlugin(),
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NoErrorsPlugin()
  ],
  module: {
    loaders: [{
      test: /\.js$/,
      loaders: ['babel-loader'],
      exclude: /node_modules/
    }]
  }
}
{% endhighlight %}

The `devtool` option is set so that if there is an error encountered during runtime, the error will specify the line number in the file it originated from, not the line in the bundled file.

The `entry` array specifies the "starting point" for webpack to start compiling your app. Generally, this will be your `index.js` or `client.js` file which initializes your app and renders it to the DOM. In the case where you'd like to use hot module reloading, you need to also specify the "hot module entry point". I believe it will be `'webpack-hot-middleware/client'` for every project. 

The `output` option specifies the output location of the bundled, compiled file. Generally, this will be the folder that you will be serving to the client, such as a `dist` or `public` folder. When you run `webpack` from the command line in your project folder, it will enter your app at the entry point, traverse the module dependencies, compile a bundled JS file, and put it into the `path` folder with the `filename` you specify. I will explain the `publicPath` option in the next section. 

The `plugins` options shown above are required for hot module reloading. The `module` config is where you specify the specific compiler options you'd like to use for each file type. In this case, we set the `test` option to find all `.js` files, and then use `babel-loader` to act on those files.  You can add more `loaders` as needed. For example, you will likely want to add a loader for CSS files.

#### webpack + server file

Here's how we implement webpack and HMR into our server file: 

{% highlight javascript %}
// server.js
var config = require('../webpack.config')
var express = require('express');
var webpack = require('webpack')
var webpackDevMiddleware = require('webpack-dev-middleware')
var webpackHotMiddleware = require('webpack-hot-middleware')

var app = express();

var compiler = webpack(config);
app.use(webpackDevMiddleware(compiler, { noInfo: true, publicPath: config.output.publicPath }));
app.use(webpackHotMiddleware(compiler));

// ... rest of server config
{% endhighlight %}

We are setting up our server to `app.use` webpack as middleware with `webpack-dev-middleware` and `webpack-hot-middleware`. As you can see, the `webpack-dev-middleware` references the `publicPath` option from our webpack config file. This means that webpack will compile the bundled file and serve it up at the `publicPath` for you.

So, let's say you set up your express server to serve your app at port 3000. You will be testing your app at `http://localhost:3000`. Since I have my `publicPath` set to be `'/'`, it will place `bundle.js` into `http://localhost:3000/`. Thus, my `index.html` will load `bundle.js` like so:

{% highlight html %}
<script src="bundle.js"></script>
{% endhighlight %}

Let's say we had set `publicPath` to be `'/public'`. Then, webpack would serve up `bundle.js` at http://localhost:3000/public and our `index.html` would have to load `bundle.js` like so:

{% highlight html %}
<script src="public/bundle.js"></script>
{% endhighlight %}

#### Babel config

Since we're using Babel to compile our code (from the `babel-loader` option in the `webpack.config.js`), you'll need to add a `.babelrc` to the root of your project. The `.babelrc` file specifies options for how Babel should compile your code. With Babel v5.x your `.babelrc` should look like this:

{% highlight json %}
{
  "stage": 2,
  "env": {
    "development": {
      "plugins": [
        "react-transform"
      ],
      "extra": {
        "react-transform": {
          "transforms": [{
            "transform": "react-transform-hmr",
            "imports": ["react"],
            "locals":  ["module"]
          }]
        }
      }
    }
  }
}
{% endhighlight %}

The `plugins` option specifies that we want to use the `babel-plugin-react-transform` library to enable the React components to be transformed when compiled. We specify *how* we want it to be transformed in the `extra` option; namely, we want to use the `react-transform-hmr` library so that we can hot module reload our components. 

#### Starting your app

Personally, I enjoy using nodemon to serve my apps on my local machine. nodemon "watches" your files and will automatically restart the server upon any changes. However, we don't want it watching *every* file, since any changes to the components will be hot module reloaded and will not need to restart the server. Luckily, we can specify files or directories for nodemon to ignore. For example, if you have React components in a directory called "components", you can start your server by running:

{% highlight bash %}
$ nodemon server.js --ignore components
{% endhighlight %}


#### Wrap up

Webpack can be confusing when you first start working with it. It is a great tool that dramatically improves a developer's workflow and app structure. However, it takes care of a lot of stuff under the hood, and so can seem mysterious at times and hard to customize. Add this to the fact that a lot of the plugins and helper libraries are in constant flux (pun intended), you now have a potentially headache-inducing set up on your hands. Try out the setup in this blog (derived from this <a href="https://github.com/kweiberth/react-redux-todo-demo" target="_blank">simple todo list demo</a>), or try to mimic (not just copy and paste) a set up from another example. Either way, do not give up! Seeing your components update immediately in the browser is an amazing feeling. Using Redux and React is an amazing feeling. Getting it (finally) working in the first place... well, that is a priceless feeling.
