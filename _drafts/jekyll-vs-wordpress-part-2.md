---
layout: post
title:  "jekyll vs wordpress part 2"
date:   2015-08-27 10:03:13
comments: true
categories: programming blog
---

I commend you if you read the entirety of part 1, and still want more! You are well on your way to setting up your first blog, which I may or may not read. Hell, maybe no one reads it, but that's not why we blog. We blog because there's a crap ton of stuff we *should* be doing, but just don't want to. Well, at least that's why I do it. Sometimes. Most of the time. Oh yeah, and to express yourself, organize your thoughts, yada yada yada. 

In the first part of this blog, I discussed how WordPress allows you to create a dynamic website or blog, whereas Jekyll is a tool for creating a static site. Here's a quick recap of the advantages of each:

##### WordPress
1. Easy to set up with little to no programming or computer skills
2. Allows you to add functionality, such as comments to your posts
3. Really, really, really large community of WordPress users
4. Backend (server) code can be private

##### Jekyll
1. Need some basic computer/programming skills to get up and running
2. Fully customizable with very little pre-defined layout or structure
3. Fast load times since it is a static site
4. All of the website's files and your post are source-controlled in git
5. Free hosting on Github Pages (will explain more in this post)
6. More secure (less chance of being hacked since there is no database to hack into!)

If you are interested in setting up your own blog and think Jekyll could be the right solution for you, then read on. Or, if you've got a lot of other stuff to do but would much rather procrastinate, read on!

#### Getting started

Here is a link to Jekyll's official documentation, which is a great resource. If you get stuck at all, you should consult these docs. If there is something that you don't understand in one of these pages, google it and get unstuck. Then, repeat. Welcome to how I've learned how to program!

Some of the basic concepts, such as the command line and git version control, are a bit out of the scope of this post. I will briefly explain the concepts you need to know, but here is a collection of some great resources for learning the basics:

- [command line](http://mac.appstorm.net/how-to/utilities-how-to/how-to-use-terminal-the-basics/)
- [Codeacademy command line (free interactive tutorial)](https://www.codecademy.com/courses/learn-the-command-line)
- [what is git?](https://git-scm.com/book/en/v2/Getting-Started-Git-Basics)
- [installing git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [basic git commands](https://www.atlassian.com/git/tutorials/setting-up-a-repository) 
- [git (interactive tutorial)](http://gitimmersion.com/)

Also, before you begin, you should make sure you have all of the requirements installed on your computer that Jekyll needs in order to work properly. You can [find out what those are here](http://jekyllrb.com/docs/installation/).

#### Okay, moving on!

If you feel that you have no idea what is going on at this point, it's okay! Don't throw your computer out the window or, worse, think that you aren't cut out for this. Learning how a computer works and how to manipulate it is a weird and foreign concept to us mere mortals. However, much like learning another language, the more you practice and the more time you give it (get some sleep to let it sink in), the easier it gets. Learning computers and programming is not rocket science. Sure, at a high level it can be used to *control* rockets, but the building blocks of computers and programming require logic and organization. Not fancy math, not magic, and definitely not rocket science. The power is within you! But if you feel stumped, contact me and I will (do my best to) help you get unstuck. Remember, I'm still basically a noob, too.

Now that you're an expert, and can't wait to use the command line to use `mv filename.txt new-filename.txt` instead of the mouse to rename your file, it's time to dive into creating a Jekyll blog.

#### Install Jekyll

Jekyll is a tool written with the programming language called Ruby. So, naturally, you need to make sure you have Ruby installed on your computer (insert download site). RubyGems is a package manager for, well, Ruby packages. In this case, Jekyll is a Ruby package. One of the easiest ways to install and update packages on your computer is to use package managers. So, make sure you have RubyGems installed (insert link).

To check if you have both Ruby and RubyGems (or, for short, gem) installed, or that they both installed correctly, head to your command line and check their version numbers. To access your command line from a Mac, open the Terminal app. An easy way to do this is to press `command + space`, and then type in "Terminal". 

Then, execute the following two commands:

{% highlight bash %}
$ ruby --version
ruby 2.2.2p95 (2015-04-13 revision 50295) [x86_64-darwin14]
$ gems --version
2.4.8
{% endhighlight %}

Let's briefly go over what's going here. You are calling the command `ruby`, and then passing in the parameter `--version` to get the version number of the Ruby that's installed on your computer. In the case you don't have Ruby installed, your computer will not find any `ruby` command to execute, and you will see a response like `-bash: ruby: command not found`.  

Okay, you've got Ruby and RubyGems installed and ready to go. Let's make them do some work! Run this command:

{% highlight bash %}
$ gem install jekyll
{% endhighlight %}

Here, you're calling the `gem` command again, but this time you're adding a more specific command that `gem` can execute; namely, `install`. The `gem install` command wants to install a Ruby package to your computer, so we tell it to install Jekyll by adding `jekyll` to the end. If all goes well, Jekyll will be installed and will be available to you to use. Fingers crossed it doesn't decided to hyde somewhere you can't find it!

Now that Jekyll is installed, it is available to you to use. Much like Ruby and RubyGems had `ruby` and `gem` available as commands, Jekyll has the `jekyll` command. Let's see how to use the `jekyll` command to create the scaffold for your new site in the next section.








with disqus, could be hard to migrate existing wordpress comments
