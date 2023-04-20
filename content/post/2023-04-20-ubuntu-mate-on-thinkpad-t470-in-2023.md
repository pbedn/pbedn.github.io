+++
title = "Ubuntu Mate 22.04 on Thinkpad T470"
date = "2023-04-20"
draft = false
tags =  ["ubuntu", "thinkpad", "rambling"]
+++

Time to celebrate, today Ubuntu 23.04 is officially available, so time for a blog entry. For Ubuntu Mate users, not much news, except for some cosmetics and AI-generated wallpapers. I am writing this while using Ubuntu Mate 22.04. 
Some issues I had on my Thinkpad T470 with dual-boot with Windows 10.
In the end, some complaints about Linux in general. 

<!--more-->

### Ubuntu Mate 22.04 on Thinkpad T470 issues

**Annoying clicking sound from speakers**

```
Verify the sound card's power_save parameter:
$ cat /sys/module/snd_hda_intel/parameters/power_save

If it returns 1 persist that configuration:
$ echo "options snd_hda_intel power_save=0" | sudo tee -a /etc/modprobe.d/audio_disable_powersave.conf

```

* Original solution - [StackOverflow](https://askubuntu.com/questions/1230833/annoying-click-popping-sound-on-ubuntu-20-04)


**Annoying error with Marco package**

On startup message shows: Sorry Ubuntu 22.04 has experienced an internal error.

Nothing can do about it, but wait for packages to be updated.

**Wrong time display in when LC_TIME is not en_EN.UTF-8**

That is an issue with Ayatana Date Time indicator and has been raised, and apparently fixed. However, when I tried to build the newest version locally I got some dependency issues. So nothing we can do, but wait for an official fix in distribution.

* Link to [Github Issue](https://github.com/AyatanaIndicators/ayatana-indicator-datetime/issues/28)
* Link to [Launchapd tracker](https://bugs.launchpad.net/ubuntu/+source/ayatana-indicator-datetime/+bug/1906238)

**Problem with Bluetooth not finding Creative Jam v1 headphones**

I had this initially because I did install Ubuntu without extra codecs and with a "minimal" set of applications.. wrong. I see now that unfortunately, the best is to install everything that comes with the distro, together with all extra non-free gnu stuff. And magic, the headphones are working fine.


---

### Linux Community

What about me? I really tried to use Linux, and for the last 15 years, I was switching back and forth. Mostly I had just Windows with Ubuntu on VirtualBox, more recently I tried WSL. Sometimes I had dual-boot with some Ubuntu flavour, and this is my current setup. I wanted Linux but did not have the motivation to fight over many technical issues, with battery, power, keyboard, sound, etc. Additionally, gaming on Linux is still almost nonexistent, though recent years opened some great possibilities thanks to Valve & Proton. And I am a heavy gamer from time to time. With Ubuntu 23.04 systemd is coming to WSL, so maybe it will become really capable now. So that is one side, technical.

Another thing, is I have a love-hate feeling about the Linux community. I would see more projects-distros-desktops come together, align somehow and merge. Only through the numbers, the Linux might grow and Linux Desktop be visible as a true partner to hardware and application developers. Serwer side this already happened, and that is great. But the rest is a pain. Of course the common answer to "Why there are so many Linux distributions, and people are diverging even more, forking, disagreeing, creating new?" Because they can... And that is the reason, MacOS has the second share in the market, not a penguin.

What if all "deb" communities would merge? Old Debian father would say goodbye. Ubuntu child would separate from Canonical and be under Linux Foundation. The rest would come back to Ubuntu. There would be one desktop environment for older computers (merge of lxde, lxqt, xfce, mate), and one for newer (merge of gnome, kde, cinnamon, budgie etc.). How do merge? Stop current feature development, all groups meet as many times as necessary, create a new project by taking all that is good from many, and while still providing only basic maintenance to old projects, work united on one future. And the such dream will never be possible. 

One of the most iconic cars in history - the Ford Model T. Quote: "You can have Ford in any colour as long as it is black". 

One Windows, most people dislike it, everybody is using it. End of rambling.
