+++
title = "Journey around Ubuntu distributions in 2023."
date = "2023-04515"
draft = false
tags =  ["ubuntu", "thinkpad", "thoughts"]
+++

It is so funny, I always start looking at alternatives just to come back to a "one-true-fits-all" solution. Welcome back Good Old/New Gnome with Ubuntu 22.04-23.04.

<!--more-->

Last month I installed and tried a few Linux distributions inside VirtualBox and on real machine: Lubuntu, Linux Mint with Cinnamon, Kubuntu, Ubuntu Mate, KDE Neon and Ubuntu.

Thoughts about distributions I tested:

#### VBox: Lubuntu 22.04

* This was my choice of Linux for VirtualBox for many years, because of light resource dependency and small light app selection.
* However I always preferred another terminal, and after the LXDE-LXQt switch, I left as well.

#### VBox: Linux Mint with Cinnamon

* Looks ok, very similar to Mate and Lubuntu, however not sure why I would be using it instead.
* Feels more alive which is a plus but from the main website only thing I remember is that Mint is asking for donations.
* Plus for not having a snap.

#### Thinkpad: Ubuntu Mate 22.04

* I am Yerba Mate lover - what to say.
* Very pragmatic desktop, for very focused people.
* Annoying new errors popping on after login.
* A little undermaintained, feels stale.
* This is however my current choice for VirtualBox Linux, as not so heavy and nice overall.

#### VBox: Kubuntu 22.04

* KDE Plasma 5.24 (https://kde.org/announcements/plasma/5/5.24.0/).
* Looks polished, similar experience to Windows, lots of customization options, maybe too much.
* No newest KDE Plasma 5.27.
* Not so-great monitor scaling option.
* I made a mistake and installed the full distro with all apps, and this was too much for me. 

#### Thinkpad: KDE Neon

* KDE Plasma 5.27 and newer.
* Very minimal set of installed apps, this is really good.
* Very annoying constant updates of software.
* Weak point - KDE Discover app.
* No apt-get, dpkg etc.

#### Thinkpad: Ubuntu 22.04

* Gnome version - 42 (https://release.gnome.org/42/)

* Everything is great, looks simple.
* There is no Light/Dark theme swither in the menu, and existing Gnome extensions do not play nicely with modified Ubuntu Gnome. I spent some hours reading forums, checking code and even thought about writing something by myself, and then I found Gnome 43.

#### Thinkpad: Ubuntu 22.10

* Gnome version - 43 (https://release.gnome.org/43/)
* Changing of style is in the menu, great news, without hesitation, I did upgrade.
* Switch from 22.04 to 22.10 was non-problematic. I did it by changing an option in "Software & Updates" to notify me "For any new version".
* One surprising advantage of not installing fresh but upgrading was that I still have the Terminal app, and not the Console that was introduced in Gnome 43. Lucky me.     

#### Thinkpad: Ubuntu 23.04

* Gnome version - 44 (https://release.gnome.org/44/)
* Honestly I did not have to switch again but I feel staying in versions ending in *.10 is bad. 
* This time switch from 22.10 to 23.04 did not finish happily, I had to rerun it and click "Partial upgrade", after that however everything is working as expected.

---

Overall I think the current Gnome desktop looks very nice. The experience and simple philosophy feel good. KDE for me was a bit too much. I see that I am the type of user that wants to have a good out-of-the-box experience, without spending much time on customisation (never was into unixporn ;). And though I like to stay on the newest software versions, I find the release cycle of Ubuntu twice per year to be enough. Rolling distributions or desktops without good snapshots and backup could be too risky and I prefer to outsource bug testing for paid QA engineers.
