---
layout: post
title:  "recursion in javascript: a case study"
date:   2015-09-04 13:03:13
comments: true
---
[Eloquent JavaScript](http://eloquentjavascript.net) is a wonderful resource for learning JavaScript.  If you are currently teaching yourself JavaScript, you should check it out. Seriously.

However, not everything in there is easy to understand on the first go around. In chapter 5, the author introduces the concept of recursive programming. Recursion is a difficult concept to grasp at first, but can be a vitally useful tool for any programmer. As an example of recursion, the author presents us with a toy problem that deals with parsing a family tree to calculate the amount of shared DNA between two individuals. This blog post is an explanation/tutorial that breaks down this problem and aims to help you understand it a bit better. You should read through [chapter 5](http://eloquentjavascript.net/05_higher_order.html) first, and if you can't fully understand how the `sharedDNA` recursion example is working, then this will (hopefully) help you out :)

First, you can head over to [the code sandbox](http://eloquentjavascript.net/code/) for Eloquent JavaScript. Here, you can select chapter 5: higher-order functions and then you can play around with the code. Basically, this loads the objects and functions for the chapter. It's cool to work in this "sandbox", but I actually find it easier to work locally on your computer. It will also help us learn how to do this stuff in a more real-world environment.

On the sandbox page, download the .zip file that contains the code for chapter 5. Unzip it by double clicking it and opening it. 

Note: This tutorial is based on the 2nd edition of the book. If the files aren't the same as I describe them or there's a new version of the book, it's possible that the content has been updated or revised. You can download the files I used to write this tutorial from my [github](https://github.com/kweiberth/05_higher_order).

Okay, in this unzipped folder we have the following files:

{% highlight bash %}
05_higher_order
├── code
│   ├── ancestry.js
│   ├── chapter
│   │   └── 05_higher_order.js
│   ├── intro.js
│   └── load.js
├── index.html
└── run_with_node.js
{% endhighlight %}

Open up `index.html` in a text editor. Make sure it is a plain-text editor, not a rich-text editor. Examples of plain-text editors are Sublime Text, Atom, and TextMate. You'll see that this file looks like this:

{% highlight html %}
<!doctype html>
<script src="code/ancestry.js"></script>
<script src="code/chapter/05_higher_order.js"></script>
<script src="code/intro.js"></script>

<body><script>
var ph = byName["Philibert Haverbeke"];
console.log(reduceAncestors(ph, sharedDNA, 0) / 4);
</script></body>
{% endhighlight %}

When you open this file in a browser, the browser will load those three javascript files linked in the first three `script` tags. Then, the browser will load the javascript written between the `<body>` and `<script>` tags. So, in this case, the variable `ph` is assigned the person object for Philibert Haverbeke. Then, the browser will `console.log` the result of `reduceAncestors(ph, sharedDNA, 0) / 4`. This dude, Philibert Haverbeke, whose object is referenced by the variable `ph`, is the author's grandfather. So what whatever number we get from `reduceAncestors(ph, sharedDNA, 0)`, will be the shared DNA *for his grandfather*. Hence, we divide that number by 4 (two generations away, each generation cuts the DNA by half) and we should have our answer for the author himself. 

Okay, so if we open `index.html` in Chrome (or any web browser, but really, just use Chrome!), Chrome will load the javascript files, then log the result from `console.log(reduceAncestors(ph, sharedDNA, 0) / 4);`. Looks like it is `0.00048828125`. In Chrome, to see the console and what it logs out, press `option + command + J` and make sure you've selected the 'Console' tab. Here, you can see what the console is logging and also interact with the environment. If you type in `ancestry[0].name` and hit Enter, it should return the name of the first person in the `ancestry` array. You can do this because you are interacting with the console in the environment where all those javascript files were loaded.

Now let's take a look at the files it's loading. Take a look at `ancestry.js`. This defines an array (in JSON format) called `ANCESTRY_FILE` which contains a bunch of person objects. Importantly, each person object has `name`, `mother` and `father` properties (among other properties). The value of each of these three properties is a string (the correspoding names). 

Cool, so this is how our browser has access to the `ANCESTRY_FILE` array. Let's look at the `05_higher_order.js` file now. The first line parses the `ANCESTRY_FILE` JSON and creates an array of the data called `ancestry`. 

The next lines are:

{% highlight javascript %}
var byName = {};
ancestry.forEach(function(person) {
  byName[person.name] = person;
});
{% endhighlight %}

This creates an object called `byName`, and populates it with a property for each person in ancestry. Each property's key is a string of the person's name. Each value is that person's person object. This allows us to access someone's person object by their name. This is useful since we will have to calculate DNA based on someone's mother and father. We can access the name of someone's mother by looking in his/her person object for the `"mother"` property, which will look like `"mother": "Mother's Name"`. Then we can use the `"Mother's Name"` string and acccess the mother's person object in `byName`. Repeat for the mother's mother, and so on and so on.

If you notice in `05_higher_order.js`, the `byName` function is defined twice. You can delete the second one near the bottom. After deleting it, save the file, and refresh the `index.html` in the browser. It should still log the correct result (`0.00048828125`).

The `average` function in this file is also unnecessary here since it was used for other examples in this chapter. Delete it, save the file, refresh the page, and make sure it still logs the correct result.

Now, the rest of the file defines the `reduceAncestors` and `sharedDNA` functions. We'll get to them in a minute. First, let's think about how the browser knows what to run, since these are just the definitions of the functions. Nowhere in this file is there a call to execute either of these functions. Remember how the `index.html` had a few lines of script that it was running? Well, this is the code that is getting executed when you load or refresh `index.html` in the browser:

{% highlight html %}
<body><script>
var ph = byName["Philibert Haverbeke"];
console.log(reduceAncestors(ph, sharedDNA, 0) / 4);
</script></body>
{% endhighlight %}

For me, it's easier when I can see everything together, so copy the javascript code here and paste it at the end of the `05_higher_order.js`. Then, delete this code from `index.html`. Save both the `index.html` and `05_higher_order.js` files and refresh the page in the browser. You should still see the `0.00048828125` logged to the console.  This is because the `console.log(reduceAncestors(ph, sharedDNA, 0) / 4);` is still being executed, it's just now located in the same file (`05_higher_order.js`) as the function defintions.

Okay, now let's take a look at the `05_higher_order.js` file. The `ph` variable is being assigned the person object for Philibert Haverbeke. Then it's calling the `reduceAncestors` function, and passing in this person object along with the `sharedDNA` function. We provide `0` as a default value in case there is a mother or father that is listed in someone's person object but is not part of the dataset (so we wouldn't be able to find out if they share DNA with the old dude).

Okay, now let's dig in and figure out what's going on here. First, let's add some `console.log` statements to the `sharedDNA` function to try and help us understand what's going on. Add to the `sharedDNA` function so that it looks like this:

{% highlight javascript %}
function sharedDNA(person, fromMother, fromFather) {
  if (person.name == "Pauwels van Haverbeke") {
    console.log("landed on Pauwels");
    return 1;
  } else {
    console.log("current person: " + person.name);
    console.log("from Mother: " + fromMother);
    console.log("from Father: " + fromFather);
    return (fromMother + fromFather) / 2;
  }
}
{% endhighlight %}

Note here I changed the syntax a bit with the brackets around the if/else clause by using brackets for each condition, and putting the else on the same line as the end bracket. Might be better, might just be personal preference. I think it's easier to read.

Now we will know when the `sharedDNA` function is called with Pauwels, the really old dude with whom we're comparing DNA. However, we only get to Pauwels at the very end of traversing the family tree, so most of the time that `sharedDNA` is called, the person with whom we're querying is not Pauwels. So, in the else clause, I log the name of the person with whom we're querying, and also I log the `fromMother` and `fromFather` parameters. Now, if you look in the `valueFor` function within the `reduceAncestors` function, it is calling this `sharedDNA` function with the statement `return f(person, valueFor(byName[person.mother]), valueFor(byName[person.father]))`. `f` is just the second parameter of `reduceAncestors`, which is `sharedDNA` since that's what we originally pass in with the call `console.log(reduceAncestors(ph, sharedDNA, 0) / 4);`. 

So when we `console.log` the `fromMother` parameter, we're actually logging what was passed in to this parameter, which as you can see from `return f(person, valueFor(byName[person.mother]), valueFor(byName[person.father]))` is actually another call to `valueFor`, this time passing in the mother's person object. So, this is the recursive aspect of this problem!

What I think gets a bit confusing is how `sharedDNA` can return a number. It seems like it just keeps retruning people, right? Well, now that we have the `console.log` statements, let's try out some easy examples and see what happens at each step.

Comment out the original calls to these function, so that the bottom of the file looks like:

{% highlight javascript %}
// var ph = byName["Philibert Haverbeke"];
// console.log(reduceAncestors(ph, sharedDNA, 0) / 4);
{% endhighlight %}

If you save the file and refresh the page in the browser, there should be nothing logged to the console, because we haven't called anything. Let's try out another, simpler call to `reduceAncestors`. Put the following code at the end of the file:

{% highlight javascript %}
var pauwels = byName["Pauwels van Haverbeke"];
console.log(reduceAncestors(pauwels, sharedDNA, 0));
{% endhighlight %}

Okay, so here we're pulling out the person object for Pauwels, the really old dude! Then, we are going to log the result of calling `reduceAncestors` with Pauwels, the `sharedDNA` function (but now complete with `console.log` statements!), and the default of `0` if we get to someone who is not in the data set. Notice we're no longer dividing by 4, because we don't have the extra step of the person with whom we're querying being a grandson of the person in the dataset.  The good thing: we already know the answer to question "How much DNA does Pauwels share with Pauwels?" 100%! So, 1.

Now, before running this code (by refreshing the browser), let's try and guess what is going to happen. We call `reduceAncestors`, which defines the function `valueFor`, but doesn't yet call it. Then, it does call it with `valueFor(person)`. Since person in this case is not null, we will head into the else clause. Now we call `return f(person, valueFor(byName[person.mother]), valueFor(byName[person.father]))`. `f` is `sharedDNA`. Pauwel's mother is null and his father is N. van Haverbeke. So, the call then becomes: 

{% highlight javascript %}
return sharedDNA({"Pauwel's": object}, valueFor(byName[null]), valueFor(byName["N. van Haverbeke"]));
{% endhighlight %}

Now, `sharedDNA` wants to return you your answer, but it needs to wait for `valueFor` to return for the second and third paramters. So what will those return? Well, since Pauwel's father is not in the ancestry array, the expression `byName["N. van Haverbeke"]` will evaluate to `null`, as will `byName[null]`. So both parameters will be `valueFor(null)`. Then, the `if` statement in these two calls to `valueFor`,

{% highlight javascript %}
if (person == null) {
      return defaultValue;
}
{% endhighlight %}

will evaluate to `true`, and the return values will each be `defaultValue` (which is `0`, which we passed in at the original call to `reduceAncestors`). Now, the interpreter can continue with the call to `sharedDNA`, yet with the returned values it now looks like this:

{% highlight javascript %}
return sharedDNA({"Pauwel's": object}, 0, 0);
{% endhighlight %}

Now we head into the `sharedDNA` function, and get to:

{% highlight javascript %}
if (person.name == "Pauwels van Haverbeke") {
    console.log("Landed on Pauwels");
    return 1;
} 
{% endhighlight %}

In this case, yes! It's true, so we will log `"landed on Pauwels"` and return `1`. This returned value of 1 is returned to the call of `return valueFor(pauwels)` within `reduceAncestors`, so our original call to `reduceAncestors` returns 1. Thus, our original statement of `console.log(reduceAncestors(pauwels, sharedDNA, 0));` will log `1` to the console.

Make sure the file is saved, refresh the browser, and make sure this worked.

Okay, so let's do one more example. I looked in the ancestor array and found a child of Pauwels, named "Lieven van Haverbeke". A child will share 1/2 of his/her (is Lieven a guy or a girl???) DNA with Pauwel, so we should get `0.5` as our final return result. Comment out the calls from before like so:

{% highlight javascript %}
// var pauwels = byName["Pauwels van Haverbeke"];
// console.log(reduceAncestors(pauwels, sharedDNA, 0));
{% endhighlight %}

And add this to the bottom of the file:

{% highlight javascript %}
var pauwelsChild = byName["Lieven van Haverbeke"];
console.log(reduceAncestors(pauwelsChild, sharedDNA, 0));
{% endhighlight %}

Now, save the file, refresh the browser and check out the console. It should look like this: 

{% highlight bash %}
current person: Lievijne Jans
from Mother: 0
from Father: 0
landed on Pauwels
current person: Lieven van Haverbeke
from Mother: 0
from Father: 1
0.5
{% endhighlight %}

I'll leave it to you to go through it step by step, but hopefully it's getting clearer. Lieven is the person we call the original function with. So, one of the last calls to return is this:

{% highlight javascript %}
return sharedDNA({"Lieven's": object}, valueFor(byName[person.mother]), valueFor(byName[person.father]));
{% endhighlight %}

By filling it in with Lieven's mother and father information, this becomes:

{% highlight javascript %}
return sharedDNA({"Lieven's": object}, valueFor(byName["Lievijne Jans"]), valueFor(byName["Pauwels van Haverbeke"]));
{% endhighlight %}

We know `valueFor(byName["Pauwels van Haverbeke"])` returns 1. `valueFor(byName["Lievijne Jans"])` will call `sharedDNA` with Lievijne's mother and father, neither of whom are in the dataset. So they are `null` and each of those two calls will return 0, and be aggregated by `sharedDNA` into a return of `(0 + 0) / 2`, which is 0. So, it becomes `sharedDNA({"Lieven's": object}, 0, 1)` and eventually returns `(0 + 1) / 2;`, or `0.5`.

Basically, `sharedDNA` waits for the calls to `valueFor` for mother and father to return. But remember, `valueFor` is calling `sharedDNA` since that's the function we passed in. So, in essence, `sharedDNA` waits for itself to evaulate each parent, which in turn needs to evaulate each parent. This creates a tree of calls, until it hits null (a family member not in the data set) or Pauwels, and returns 0 or 1. Then, the 0 or 1 is returned back, and the `sharedDNA` call from which it came will evaluate. As the outermost tree "branches" start returning 0 or 1, they are combining numbers together (and dividing by 2). Hence, a 1 from Pauwels keeps geting diluted by 1/2. Hence, you have smaller and smaller numbers.

It's still hard sometimes to precisely wrap my head around recursion. I decided to write this explanation out fully and it really helped me to think about. Recursion is just simply not how our brains are wired to think, and so it takes time and practice. My general advice in a situation like this is to play around with `console.log` to write out variables at certain points in the execution of the code. There are more elegant ways of doing this using `debugger` statements with Chrome's console, but I was having trouble with it and this method works just fine.

My other general suggestion is to think about the base case (in this example, Pauwels), and then work your way up to understanding the more complicated cases that need to make a ton of recursive calls before returning.

Hope this helped!
