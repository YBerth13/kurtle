---
layout: post
title:  "focusing the chrome console with keyboard shortcuts"
date:   2015-10-11 03:03:13
comments: true
---

I'm a lazy bum. Actually, I'm only a software engineer, and the laziness and bumminess comes along for free. This means that I will go to great lengths (once) in order to, for the rest of eternity, continue my laziness.

Recently, I noticed that I was expending an exorbitant amount of time moving my right hand from the keyboard to the trackpad. Sometimes, when I was working at my desk, I was moving my right hand *all the way to the mouse*. And it was just to open an application, or click a link in Chrome. There had to be a better way.

As it turns out, there is. It's called the keyboard. For the most part, your keyboard is capable of doing anything on your computer. So, I committed to using keyboard shortcuts for everything. Luckily, at Hack Reactor, I attended an awesome talk/demo on keyboard shortcuts by Josh Wyatt. Josh has compiled a [great resource](https://github.com/joshwyatt/keyboard_shortcuts) for many of the most useful keyboard shortcuts for programmers. During the demo, during which he displayed amazing keyboard-ninja skills, he introduced us to [Vimium](https://vimium.github.io/), a Chrome extension that allows keyboard-driven navigation of Chrome. It is awesome. With simple shortcuts you can open links, focus and cycle through text field inputs, scroll slowly or quickly, copy the current URL, and more, all from the comfort of the home row. Hitting `?`, which brings up a menu showing all of Vimium's shortcuts, is likely the most useful shortcut as you begin to learn how to be a keyboard.

Unfortunately, the first few days of using Vimium feel like *more* work instead of less. This is a common feeling when learning keyboard shortcuts, and the only remedy is to commit to the keyboard. It will get better. In the long run, you will be lazy. So, I installed Vimium, and decided I would say goodbye to the trackpad.

However, I quickly ran into an issue while using Vimium and Chrome's developer tools at the same time. If I brought the focus into Chrome's console (by hitting `cmd + option + J`), worked in the console, and then wanted to bring focus back to the web page and use Vimium to navigate the page, I had to use my mouse. This was bogus! There had to be a keyboard shortcut for it, right?

Turns out, there really isn't. I'm not the first person to long for such functionality, but it seems that Google has not implemented a keyboard shortcut to solve this task. I did, however, find some solutions that worked *pretty* well. The following sections go through some common cases and solutions for using your keyboard to switch focus between Chrome's console and the current web page.

#### Switch focus from web page to (currently unopened) Chrome console

If you are on a web page and would like to open up and switch focus to Chrome's console, you can use `cmd + option + J`.

#### Switch focus from web page to (currently opened) Chrome console

If the Chrome console is currently open, and you'd like to switch the focus back to the console from the web page you're browsing, there are a couple of options you have, depending on how you like to use the console:

##### You use the console in the same window as the web page

Unfortunately, there is no keyboard shortcut specifically for this task. However, you can bring focus into the console by hitting `cmd + shift + C` twice. Hitting it only once will bring focus into the console, but will also initiate the "inspect element" mode on the web page itself. Hitting it a second time toggles off this "inspect element" mode, and focus stays in the console.

##### You use the console in a separate window

If you use the console in a separate window, you can toggle between an application's (in this case, Chrome's) windows by holding down `cmd` and tapping `` `. When the window you'd like to use is brought into focus, let go of `cmd`.  The one downfall of this method is that you will have to toggle through all of Chrome's open windows, which could be a lot depending on how you work. Note: this same method can be used for moving the focus back to the console.

#### Switch focus from Chrome's console to web page 

This is the simple task that sent me down the rabbit hole of researching keyboard shortcuts and focusing Chrome's console. There are a couple solutions, none of which are perfect.

##### Close Chrome's console

This is the easiest, but likely worst, solution. If your current focus is in the console, and you'd like to return the focus to the web page, you can hit `cmd + option + J` to close the console. To get back to the console, use the same shortcut to reopen it. 

##### Keep Chrome's console open and in the same window

The best solution I found for this task was presented in a post on [Stack Overflow](http://stackoverflow.com/questions/12823333/keyboard-shortcut-to-switch-focus-from-web-developer-tools-to-page-in-chrome-on/30567119#30567119). While the focus is in the console, hit `cmd + L` to move the focus into Chrome's omnibox. Then, type `javascript:` and hit `Enter`. Note: that's javascript with a colon right after. Doing this should switch the focus back to the web page and allow for Vimium navigation. 

Obviously, this is hardly a shortcut if you have to type in `javascript:` each time. To shorten this command, you can head into Chrome's preferences (`cmd + ,`), and then head into the `Manage Search Engines` menu. At the bottom, there should be three text inputs which allow you to add a custom search engine shortcut. In the first box (called 'Add a new search engine'), type in some identifier, such as `Switch focus to web page`. In the second box (called 'Keyword'), type `j`. This is the shortcut you're setting up. In the last text box (called 'URL with %s in place of query'), type in `javascript:`. Save these settings.

Now, when you're in Chrome's console, you should be able hit `cmd + L`, then type `j`, then hit `Enter` to switch focus to the web page. 

Please consult the [Stack Overflow post](http://stackoverflow.com/questions/12823333/keyboard-shortcut-to-switch-focus-from-web-developer-tools-to-page-in-chrome-on/30567119#30567119) for more information.

#### Wrap up

Using the keyboard to switch focus from Chrome's console to the web page seems like a simple, unimportant task, but when I'm trying to be cool and not use my trackpad, it's a huge deterrent. Okay, in reality, writing about how to do this in the first place might preclude any coolness at all, but I still want to be as lazy as possible. 

In all seriousness, I've enjoyed using keyboard shortcuts as much as possible. It not only makes me looks way more proficient than I necessarily am, it also helps limit arm movement and may reduce my chances of tennis elbow and other repetitive strain injuries. 

So, roll up your sleeves and learn how to use your keyboard to do *everything*. On second thought, don't roll up your sleeves. Sounds like too much work.
