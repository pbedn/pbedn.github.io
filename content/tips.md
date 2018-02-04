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


