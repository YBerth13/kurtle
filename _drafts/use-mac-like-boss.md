---
layout: post
title:  "how to use your macbook like a boss"
date:   2015-09-02 03:03:13
comments: true
---
I first started using a Macbook in 2008 when I was heading off to college. At the time, one of the unfamililar aspects of Mac OS X was its window layout and management. On Microsoft (maybe XP at the time?) Windows, application windows could easily be toggled to full screen with the "maxmize" button, which hung out right next to "minimize" and "close". Made sense. When I started using Mac OS X, I was presented with three small, colorful circles at the top of each application window (on the wrong side I might add!). I clicked the red button... the window closed. After reopening, I tried the yellow one... the window minimized. Wow, they were really nailing it. At this point, I knew I could totally learn how to use a Mac. I just needed to maximize this window so I could quickly reorganize some files and... wait a minute, the green button didn't maximize it. Sure, the window got bigger. But not full screen-bigger. Just different-bigger. And on another window, it made it an even different different-bigger. I paid 1300 bucks for this shit?

Then, after I realized that yes, I had indeed paid $1300 for it, I decided I would immediately accept this flaw (oddity) and continue deluding myself into liking Mac OS X. After only a couple months, I was completely brainwashed and have since been a proud owner of eight Apple products. What a ride.

### There's a tool for that

Nonetheless, brainwashed or not, Macbook power-user or just casual-user, you may still get frustrated from time to time when trying to (re)size application windows. Luckily, I have a solution; let me show you a few tips and tricks that have worked great for me.

#### Step 1: Put on your spectacle(s)

The cool thing about using computers is that they can be customized to your needs. If there is a lack of functionality that you require, somebody, somewhere probably created a tool you can use to fill this gap. To me, this is one of the greatest attributes of computer programmers. In one respect, they're lazy as shit. Having to resize each window manually? Screw that. Coding an entire appliation that allows you to resize windows with keyboard shortcuts? Yeah sure, no problem.

Unsurprisingly, there are a plethora of window resizing tools out there. I've used a few, including BetterSnapTool and SizeUp. Both got the job done just fine. I've also heard great things about Divvy, which shows you a grid of your screen and allows you to place windows with you mouse in a more visual manner. To me, however, this is a little bit of overkill. I've settled on Spectacle, a free and open-source project. Let's see how to set it up.

First, head over to Spectacle's website. Clicking the large "Download Spectacle" button will download a .zip file to your computer. Unzip this folder (by double-clicking it and opening it). You should now see a file called `Spectacle.app` in the folder where the .zip was downloaded. This is the Spectacle app! To make sure it stays around, click and drag `Spectacle.app` into your `Applications` folder. 

Now head into the Applications folder and double-click `Spectacle.app` to open it and start using it! You can find out more information about the app in the [README](https://github.com/eczarny/spectacle) of the open-source project. Here are the basics:

#### Spy glasses in your status menu bar

Check the top of your screen right now. You probably have some icons on the right hand side, such as the time and date, wifi status and volume controls, among others. After opening Spectacle, you should now see a pair of spectacles chilling alongside the other icons. Click these spectacles, and you should see a menu like this: 

Here is a list of all the possible positions you can send a window. This should be pretty self explanatory. By clicking on one of these, you will send the current application window you're looking at to that location. So, maybe you want your browser to head to right side of the screen. Make sure your browser is open and you're looking at it. Then click the spectacle icon in your status menu bar, and click `Right Half`. Boom!

#### Set those prefs!

Before I show you the true power of Spectacle, let's make sure the settings are, well... set. Click the spectacle icon in your status menu bar, and click on `Preferences`. This will open up Spectacle's preferences. Most of these options are aimed at displaying, or if you want, customizing, the keyboard shortcuts for all the different positions. At the bottom, you see a nice key for what the different symbols are. I can never seems to remember these symbols, so this is useful. In the bottom right corner, click the right arrow. Here you will have two preferences you can set. The first, `Launch Spectacle at login` sets whether or not Spectacle automatically starts when your computer boots up. I keep this checked, since having to open the app when you reboot is an unnecessary step. The other preference allows you to keep the Spectacle app in the status bar (so you always see the little spectacle icon), which is useful if you don't want to remember the keyboard shortcuts and will instead use your mouse to select window positions. This is what we did above when we sent your browser to the `Right Half`.  I suggest leaving it in the status bar, but you can also make it run only in the background by setting this preference to `as a background application`.

### You've learned it, now master it

Stop being such a noob and using your mouse and status bar to change your window sizes. That takes like a whole 2 seconds. What a waste, you noob!

#### Learn some shortcuts!

Clearly, there are a lot of Spectacle keyboard shortcuts that send windows to a bunch of different locations. Don't be intimidated. Find out which positions you prefer for your windows, and then use, learn and memorize those shortcuts. Then, [feel the power](https://youtu.be/b3c1RORu_Bs)! Here's four shortcuts I use and love:

#### Command + F

The `command` key is a super important key for Mac users, as it's at the heart of most basic keyboard shortcuts. For reference, the `command` key can be abbreviated as `cmd` or symbolized with ` ` (which is printed on the key itself, too). Pushing `command` and `F` will enlarge your current window and allow it to fill the maximize size it can.

#### Option + Command + left arrow 

The `option` key is located next to the `command key`. For reference, the icon for the `option` key is ` `. Pressing `option` and `command` and `left arrow` will make your current window fill exactly the left half of the screen. 

#### Option + Command + right arrow 

This shortcut is very, very similar to the last one. I think you can figure it out. 

#### Control + Option + Command + right/left arrow

The `control` key is next to the `option` key and is referenced with the icon ` `. This shortcut will move your display to another screen (if you have another screen hooked up to your laptop). For example, if you have a large external monitor that you use for work, you may want to send a window from that screen to your laptop's screen. To do that, use this schortcut. 

### You've mastered it, now boss it 

No doubt you've been wondering this whole time: "*Hey dude, the title promises me that I'll learn how to use a my Macbook like a boss. But I don't feel very boss-like yet. In fact, I kinda feel like this has all been a waste of time.*" Ha! this was just warm ups. You've now reached the boss level.

#### Tidy up that Dock

One quirk about window sizes on a Macbook is that the dock takes up space, and does not allow a window to be resized over it. Essentially, the Dock takes up valuable real estate on your screen. Luckily, there are a couple easy steps you can take to maximize your screen's real estate for windows.

Take a look at your laptop's screen right now. What shape is it? If you guessed rectangle, you are correct! Now here's a real brain teaser... is it wider than it is tall, or taller than it is wide?

Spoiler alert: it's much wider than it is tall. So, if you have your dock on the bottom, you are decresing the (already smaller) amount of vertical space you can use for windows. To improve this, head into your `System Preferences`, and click on `Dock`. Change the location of the Dock to either left or right, whichever you prefer. Left made more sense to me. While you're at it, change its size to as small as it gets. 

And voila! we've now moved the Dock into a better position and freed up as much screen real estate as possible by making it small. Test it out by Spectacle'ing a window into full screen (`option` `command` `F`), and it should be even bigger than before. Magical.

#### Get rid of the Dock

I know what you're thinking: "*Say what? I use that shit!*". Maybe. But you don't need to. Get ready to *really* feel the power. 

Open up the `Dock` panel in `System Preferences` again. Click the checkbox next to the option `Automatically hide and show the Dock`. Oh no, where did it go? Don't worry, it's still there. If you hover your mouse over the area where the dock used to be (hopefully the left or right after reading the last section!), then it will appear for you. The nice thing about this set up is that now the windows can occupy the space once taken by the dock. And if you reallllly need to use the dock, you can make it appear by moving your cursor over to it. But really, why would you do that?

#### Use Spotlight, not the Dock
