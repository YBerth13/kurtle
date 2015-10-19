---
layout: post
title:  "putting this into context with bind"
date:   2015-10-18 10:03:13
comments: true
---

One of the more difficult parts of JavaScript to understand is the keyword `this`. "This" is an extremely descriptive word, so it's hard to believe people can't grasp "this" simple concept right off the bat.

Just kidding, of course! In reality, "this" is not descriptive at all. To make it even more ambiguous, the value bound to `this` within any given function is determined only at runtime, making it a common culprit of errors. 

One common use case that involves the keyword `this` is pseudoclassical instantiation and their corresponding method calls. For example, we might define a class called `Bicycle`. `Bicycle` will be a function that produces bicycle instances (objects). The `Bicycle` method might look like something like this:

{% highlight javascript %}
var Bicycle = function(manufacturer, type) {
  this.location = 0;
  this.manufacturer = manufacturer;
  this.type = type;

  this.pedal = function() {
    this.location++;
  };
}
{% endhighlight %}

To instantiate (create) bicycles, we can run the following code: 

{% highlight javascript %}
var trek = new Bicycle('Trek', 'road');
var cannondale = new Bicycle('Cannondale', 'cyclocross');
{% endhighlight %}

We have now instantiated two new bicycle instances. Each will have a property `location` that is set to `0` when the instance is created. Each instance also has a method called `pedal` which, when invoked, will increase the `location` of that bike by 1. For example:

{% highlight javascript %}
cannondale.pedal();
cannondale.pedal();
console.log(cannondale.location); // 2
{% endhighlight %}

Notice that inside the `pedal` method we are incrementing the `position` property on `this`. So, what value is `this` bound too? Luckily, it seems that it was correctly bound to the instance itself, so in this case invoking `pedal` on the `cannondale` instance was acting on the instance itself, as intended. This is a common pattern in object-oriented (class-based) programming, and is the original intention of the `this` keyword. Generally (maybe 90% of the time), `this` gets bound to the object that is to the left of the dot of the function invocation; in this case, the `cannondale` object. However, just like everything in life, it isn't *always* so straightforward. For example, let's say we wanted to use `setTimeout` and invoke the `pedal` method on the `cannondale` instance in 2 seconds. We would run the following code:

{% highlight javascript %}
setTimeout(cannondale.pedal, 2000);
{% endhighlight %} 

As a reminder, `setTimeout` takes in two arguments: a function object and a time (in milliseconds) that you want to wait before invoking that function object. This means that we are not invoking the `pedal` method right away, nor in the same context in which this code is written. Notice how we did not pass in `cannondale.pedal()`, which would invoke the function immediately, but rather we passed in the *function itself* with `cannondale.pedal`. 

So, the code above could be rewritten (and is interpreted as):

{% highlight javascript %}
setTimeout(function() {
  this.location++;
}, 2000);
{% endhighlight %}

In 2000 milliseconds, the `pedal` function is invoked. Once it is invoked, the `this` keyword will be bound to something. We're hoping it's the `cannondale` instance. But where in the world is that `cannondale` object? How will this function know to bind `this` to it?

In short, it won't. Methods that are invoked asynchronously, either via `setTimeout` or some other means, will be invoked within the global context as opposed to within the context of the instance. When this happens, `this` is bound, for better or worse, to the `window` object. In this case, the `window` object doesn't have a `location` property, and so this code will break.

Looks like we're out of luck. You either pedal that bike right now, or you can't ride it! 

Again, just kidding! Let's explicitly bind the value of `this` to the instance on which we the `pedal` method to work. To do this, we can use the `bind` method. `bind` is a method that an be invoked on any function. Let's see how it works by returning to our example of calling `pedal` within `setTimeout`. Using `bind`, we can rewrite our example:

{% highlight javascript %}
setTimeout(cannondale.pedal.bind(cannondale), 2000);
{% endhighlight %}

This syntax may look a little odd, but here's what's going on. Remember, `cannondale.pedal` is a function:

{% highlight javascript %}
function() {
  this.location++;
}
{% endhighlight %}

`bind` takes the argument supplied to it, in this case the `cannondale` instance, and binds it to the `this` keyword within the function on which it's called. So, basically it takes the `pedal` function, and returns a function with `this` bound to the `cannondale` instance. For all intents and purposes, the function it returns looks like this:

{% highlight javascript %}
function() {
  cannondale.location++;
}
{% endhighlight %}

So, now our `setTimeout` code looks more like:

{% highlight javascript %}
setTimeout(function() {
  cannondale.location++;
}, 2000);
// wait 2000 milliseconds
console.log(cannondale.location); // 3
{% endhighlight %}

After 2000 milliseconds, the function is invoked. Since `this` was correctly bound to the `cannondale` instance after `bind` did its work, our code will correctly increment the `cannondale` instance's location property.

That's all there is to it! You are now officially a master of the JavaScript language. Okay, not really. But if you start talking about `this` and `bind` to your friends, you might seem like one. Or, at the very least, a huge nerd.

There is a lot more to learn about the `this` keyword and all of its idiosyncracies. However, if you are having trouble with an async function invocation, and the function contains `this`, it might be a good idea to start troubleshooting the value that's bound to `this` at runtime. It just might be that `bind` can be your friend. And if not, then I'll be your friend :)

I'll leave you with a bonus question to test your understanding. What will the value of `trek.location` and `cannondale.location` be when this code runs?

{% highlight javascript %}
// setting both bike instances' location props to 0
trek.location = 0;
cannondale.location = 0;
setTimeout(cannondale.pedal.bind(trek), 2000);
// wait 2000 milliseconds
console.log(cannondale.location); // ?
console.log(trek.location); // ?
{% endhighlight %}
