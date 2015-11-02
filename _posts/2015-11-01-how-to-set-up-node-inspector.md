---
layout: post
title:  "how to use node-inspector to debug node applications"
date:   2015-11-01 10:03:13
comments: true
---

Do you use `console.log` statements to debug your JavaScript? Do bears shit in the woods? Do I eat way too much Chipotle?

Look, it's okay, we can admit it. We all use `console.log` every now and again. And, in fact, it is a completely valid method for debugging. But sometimes it's just plain *inefficient*. And being a lazy programmer, that's *all* I care about. 

Back in the day, when I was just learning how to program, I had code that looked like this:

{% highlight javascript %}
// ...some code
console.log('got here');
// ...some more code
console.log('got here 2');
// ...even more code
console.log('got here 3');
// ...
{% endhighlight %}

It gets the job done. But so does Qdoba. There is a *better* way: Chrome DevTools. If you've ever written client-side JavaScript, you've likely used the DevTools to help you debug, even if you were still throwing in the occasional (or frequent) `console.log` statement. 

A problem, however, arises if you've found yourself working on server-side JavaScript in the form of a node.js application; namely, the browser's DevTools just don't work. So, it's back to the tried-and-true `console.log` statements and difficult-to-read Terminal outputs, right?

Haha! What a noob you are! Allow me to introduce you to <a href="https://github.com/node-inspector/node-inspector" target="_blank">node-inspector</a>. node-inspector is a debugger interface for node.js applications that looks and works exactly like Chrome's DevTools. Once I got node-inspector up and running, used it to trace a bug, squash it, and happily move on to more coding, all while *not once* typing `console.log`, I was sold. It was revolutionary. Let me show you the way.

#### Install node-inspector globally

You'll want to install node-inspector as a global module (since you'll be using this for everything, trust me) by running the following command in your Terminal:

{% highlight bash %}
$ npm install -g node-inspector
{% endhighlight %}

Depending on your permissions, you may need to run this with `sudo`. You can check to make sure the installation worked by running 

{% highlight bash %}
$ node-inspector -v
{% endhighlight %}

Terminal should output the version number if it installed correctly. To start node-inspector, simply run the command

{% highlight bash %}
$ node-inspector
{% endhighlight %}

You should see an output similar to this

{% highlight bash %}
Node Inspector v0.12.3
Visit http://127.0.0.1:8080/?ws=127.0.0.1:8080&port=5858 to start debugging.
{% endhighlight %}

#### Start your node app in debug mode

Normally, to run your app, you will likely navigate to your project folder and run something like 

{% highlight bash %}
$ node ./server.js
{% endhighlight %}

This command is telling node to run the `server.js` file (your filename may differ), thereby starting the server. In order to use node-inspector, open up a new tab in Terminal (to allow node-inspector to keep running), and run

{% highlight bash %}
$ node --debug ./server.js
{% endhighlight %} 

The `--debug` flag causes your node.js application to "plug-in" to node-inspector. With your server up and running, navigate to node-inspector's url in Chrome. This url can be found in the output from the `$ node-inspector` command, but will generally be `http://127.0.0.1:8080/?ws=127.0.0.1:8080&port=5858`.

Within a couple seconds, node-inspector should load with your application. You can navigate to the "Sources" tab, and should see all of your source files. You can now debug the absolute hell out of your app, just like you would with client-side JavaScript. 

#### Customizing node-inspector's port

By default, node-inspector sets up shop at port `8080`. So, you will want to choose another port for the node app itself. `3000` or `4000` are good, common choices. If you are (possibly unreasonably?) attached to `8080` as your app's port, you can easily set node-inspector's port manually by running 

{% highlight bash %}
$ node-inspector --web-port <INSERT PORT NUMBER>
{% endhighlight %} 

#### Let's make it a one-liner

Starting node-inspector in one tab in Terminal and then running your server in another tab is just *way* too much work. Instead, you can use the `&` operator, which runs two commands in parallel. So, when you want to start up your server in boss mode (aka using node-inspector), you can run 

{% highlight bash %}
$ node-inspector & node --debug ./server.js
{% endhighlight %} 

If all worked well, you should see an output similar to this:

{% highlight bash %}
Node Inspector v0.12.3
Visit http://127.0.0.1:8080/?ws=127.0.0.1:8080&port=5858 to start debugging.
Debugger listening on port 5858
Server listening on http://127.0.0.1:3000
{% endhighlight %} 

That last line, `Server listening on http://127.0.0.1:3000`, is a log statement from my `server.js` file.

#### Why aren't you using nodemon?

If you've been following along, then you've been using the command `$ node ./path/to/server.js` to start your server. This is noob status. You should really use nodemon (pronounced no demon? Or no-duh-mahn? I actually have no idea...). nodemon is command line tool for running node applications. What makes it great is that it monitors your files, and whenever one of them changes, it automatically restarts your server. To get started, install nodemon globally, just like node-inspector:

{% highlight bash %}
$ npm install -g nodemon
{% endhighlight %} 

Now, you can run your server with 

{% highlight bash %}
$ node-inspector & nodemon --debug ./server.js
{% endhighlight %} 

and should seen an output like the following

{% highlight bash %}
Node Inspector v0.12.3
Visit http://127.0.0.1:8080/?ws=127.0.0.1:8080&port=5858 to start debugging.
[nodemon] 1.8.0
[nodemon] to restart at any time, enter 'rs'
[nodemon] watching: \*.\*
[nodemon] starting 'node --debug ./server.js'
Debugger listening on port 5858
Listening on http://127.0.0.1:3000
{% endhighlight %} 


If there are certain files that you don't want nodemon to monitor, you can add an <br>`--ignore` flag, like so

{% highlight bash %}
$ node-inspector & nodemon --debug ./server.js --ignore ./path/to/ignored/file.js
{% endhighlight %} 

#### Use an npm script

Just like running two commands in two different tabs was too much work, so too is typing that whole, monstrosity of a command. Instead, let's use an npm script. To run npm scripts, simply define commands within a `"scripts"` property in the `package.json` file. In this case, let's set up a `"serve"` command, like so:

{% highlight javascript %}
// package.json
{
  "name": "awesome-app",
  "version": "0.0.1",
  "dependencies": {
    // lots of awesome deps...
  },
  "scripts": {
    "serve": "node-inspector & nodemon --debug ./server.js"
  }
}
{% endhighlight %}

Now, to start our serve and use node-inspector, we can run this one, measly command:

{% highlight bash %}
npm run serve
{% endhighlight %} 

Life is good.

#### So you use gulp? 

Well, aren't you cool (I know, because I use gulp, too). At first, I struggled to get node-inspector set up with my `gulp serve` task. Luckily, there was some great information in a <a href="https://github.com/JacksonGariety/gulp-nodemon/issues/85" target="_blank">github issue thread</a> for gulp-nodemon. This is what I have for my set up:

{% highlight javascript %}
var nodemon = require('gulp-nodemon');

gulp.task('serve', ['build:js', 'css'] function() {
  gulp.watch('sass/*.scss', ['css']);
  return nodemon({
    exec: 'node-inspector & node --debug',
    ext: 'js',
    script: './server.js'
  });
});
{% endhighlight %}

#### Wrap up

node-inspector is super cool. `console.log` statements are a perfectly valid way of debugging, but I urge you to take the time to learn how to use the DevTools/node-inspector to debug. Recently, while pair programming, my partner and I debugged an issue using node-inspector. We tracked the flow of the program through multiple functions and multiple files, dynamically setting breakpoints as we went along. At one point, we realized we hadn't written an `else` clause that was about to evaluate. We wrote the `else` clause in the debugger and the program continued without a hitch. It was revelatory. `console.log`, I've found someone better. It's name is node-inspector.
