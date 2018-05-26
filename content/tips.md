+++
date = "2017-03-12"
title = "Tricks & Tips"
draft = false
+++

**Disable bluetooth on linux with systemd**

```
sudo systemctl [start, stop, enable, disable] bluetooth.service 
```

**Disable bluetooth on startup on Ubuntu Mate 16.04**

Go to *dconf-editor->org.blueman.plugins.powermanager* and set value on *auto-power-on* to **false** 

**Wireless Bluetooth Creative Soundblaster Jam headphones not connecting in Ubuntu 16.04/18.04**
Solution found [here][creative-link]. Download file, move to /lib/firmware/brcm/ and restart.

```
wget https://s3.amazonaws.com/plugable/bin/fw-0a5c_21e8.hcd
sudo mv fw-0a5c_21e8.hcd /lib/firmware/brcm/BCM20702A0-0a5c-21e8.hcd
sudo shutdown -r now
```

Alternate file location -> [Dropbox][creative-dropbox-link]

[creative-link]: https://ubuntuforums.org/showthread.php?t=2308489
[creative-dropbox-link]: https://www.dropbox.com/s/12504w56lm3brtm/fw-0a5c_21e8.hcd?dl=0
