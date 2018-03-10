+++
title = "Ubuntu Mate on Thinkpad T470"
date = "2018-02-01"
draft = false
tags =  ["ubuntu", "thinkpad"]
+++

Update:
After recent update laptop freezes at boot!

I was looking for a new laptop for some time now, and specifically, I wanted to try ThinkPad series, based on sound reviews. Electronic equipment in Poland has a higher price compared to for example the USA, but I bought on a sale a T470 without Windows preinstalled for 1140$ (3800 PLN on 26 Jan 2018). The usual price for this model was around 5000 PLN. Anyway, I was going to install Ubuntu, with choices between Lubuntu (my long-time favourite on all virtual machines) and Ubuntu Mate (more pretty but not so heavy as default Unity or currently Gnome 3).

<!--more-->

The second consideration was to choose 16.04 LTS, especially after recent bad news for some Lenovo users who installed 17.10 and their laptop got bricked because of BIOS failure (T470 was not on 'broken' list but let's not play with the devil). I expect somewhere in late autumn to switch to 18.04 after it is tried out for some months.


My specification:

- Model: 20HES07J00
- CPU: i5-7200U
- SSD: 256GB
- RAM: 8GB
- Graphics: Intel 620
- Battery: 3 + 3

---

My first impression after few hours was that this is excellent notebook. A bit heavier than my previous Toshiba Portege Z930 (1.12 kg), this one weights 1.64 kg. Still, the keyboard is the best I have used so far, and for me, it is much better than the one in MacBook Pro 2017 13" I had a chance to test. What I have got used to is a different layout of Home and End buttons (right edge) and Ctrl first (Thinkpad has Fn).  

I have installed Ubuntu Mate 16.04 from a usb stick, prepared earlier on my older Windows 10 laptop with Rufus app. No problems during installation. I have chosen default settings for hard drive formatting, as I am not going to have two systems running. And I learned my lesson not to break Linux root system on the same day...

**Almost everything works out-of-the-box except:**

 1. Keyboard backlight was resetting to 50% each time after few seconds of inactivity or power state change
 2. Microphone key shortcut was not working
 3. Brightness had strange behaviour, changing level one step up after few seconds of inactivity but not permanently like with keyboard backlight. 
 4. Surprising quick battery discharge (max 6-7 hours for economy usage instead of 14 advertised ..)

**Reg. 1:**

Resolution found on [ubuntu-mate.community][keyboard-backlight].

Edit the file:

```
/etc/dbus-1/system.d/org.freedesktop.UPower.conf.
```

And change:

```
<allow send_destination="org.freedesktop.UPower"
send_interface="org.freedesktop.UPower.KbdBacklight"/>
```

To

```
<deny send_destination="org.freedesktop.UPower"
send_interface="org.freedesktop.UPower.KbdBacklight"/>
```

Reboot.


**Reg. 2:**

As this is a longer solution, I will provide a link to [AskUbuntu][mic-shortcut]. I have chosen the first solution marked as c-1. The only thing that is annoying - power indicator blinking when mic mute is on.

**Reg. 3:**

Add parameter to grub session. How to do it: [wiki.ubuntu][add-grub-boot-param]. 

```
acpi_backlight=none
```

[Link to original solution][brightness-level]

**Reg. 4:**

I propose to install the latest version of tlp - [link to website][tlp-manager]:

```
sudo add-apt-repository ppa:linrunner/tlp
sudo apt update
sudo apt install tlp tlp-rdw tp-smapi-dkms acpi-call-dkms
```

Start and see status

```
sudo tlp start
sudo tlp-stat
```

And diagnose usage with 
```
sudo apt install powertop
sudo powertop
```

Worse battery management on Ubuntu is something I would have to live with, in the worst case I can always buy additional external 72 Wh battery, and thanks to dual setup just swap when needed.  

[keyboard-backlight]: https://ubuntu-mate.community/t/keyboard-light-keeps-turning-on-after-login-and-or-unlock/6914/12
[mic-shortcut]: https://askubuntu.com/questions/125367/enabling-mic-mute-button-and-light-on-lenovo-thinkpads#Determining
[brightness-level]: https://github.com/mate-desktop/mate-power-manager/issues/216#issuecomment-330727162
[add-grub-boot-param]: https://wiki.ubuntu.com/Kernel/KernelBootParameters
[battery]: https://askubuntu.com/questions/756940/bad-battery-life-on-xubuntu-16-04-lenovo-t460s/757196
[tlp-manager]: http://linrunner.de/en/tlp/docs/tlp-linux-advanced-power-management.html
