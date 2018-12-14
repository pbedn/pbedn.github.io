+++
date = "2017-09-22"
title = "Best Python IDE: Sublime Text vs PyCharm vs Wing IDE"
tags = ['python', 'ide']
draft = false
+++

After three years of using Sublime Text 3, more than one year with PyCharm (Community at home and Professional at work) should I switch to Wing IDE? It has some of the best features from previous editors. Time to compare them.

<!--more-->

**Sublime Text**

I like Sublime because it is fast. Simple. It was able to open huge files when other editors just crashed. Opening time is instant. Multiple selection is fantastic and so on.. I am writing this inside Sublime as it is my default editor. After many trials with packages, and trying to make it a good IDE for Python I just settled with a few must-have. But in general more external tools, slower it gets, and even this super stable platform crashed a few times (git gutter is the worst of them). I like that plugins can be written in Python, but this type of hacking is not for me.

Plugins I use (I will not mention Package Control, I do not get why it is not installed by default?):

* Side Bar Enhancements
* Compare Side-By-Side
* Terminal

That's it. I tried Anaconda to make my Sublime a full featured IDE, like they advertised and it is.. ok (before my co-worker showed me PyCharm). I tried to install different packages manually, and I liked especially *Pylinter*. For small side projects that was enough. But when I started working with a bit bigger legacy code base, my Sublime failed. And I always wanted to have integrated terminal, well..

**PyCharm**

I resisted a long time before using it. And I think that for 80% of any developer need, it is an unnecessarily complex and CPU hungry devil. Still I am going to use it because of its powerful debugger.

Things I like:

* Free
* Configuration of environment
* Debugger

Things I dislike:

* Slow
* I can open it only once per day, as it is eating 90% of my IvyBridge i7 and I do not even know how much RAM because I just make a break..
* Searching
* Jupyter notebook functionality is terrible (who would use it inside anyway, but I like to have additional negative point here)

Conclusion. Do not bother with PyCharm if your project is small, life is too short. On the other hand, it will save you a lot of hours as did for me when I switched from pdb (and ipdb) to its debugger.

**Wing IDE**

Some reviews on the web say that it is slow. Not true, maybe older versions but the current is pretty fast. Overall it feels like a middle ground in comparison to previous editors. It has missing debugger and terminal from Sublime and runs much faster than PyCharm. Nice. And *Source Assistant* is pretty cool (in addition I like dark theme Monokai better than Darcula ;)

I did not even installed it, just using portable version (which is cool). But the debugger is average (PyCharm raised the bar really high). I had problems with viewing data inside simple dict using *Stack Data* tool and tools like *Debug Probe* and *Watch* are enabled only in Wing Pro version. So it is a pain sometimes while debugging. Well, you get what you pay for. Looks like old *print()* never dies. 

Pros:

* Sleek UI and quite fast engine
* Plugins written in Python (and lots of examples)
* This is IDE (not like Sublime - even with Anaconda)

Cons:

* Debugging is comparable to PyCharm, only in Pro version, and still not as good
* Price for Wing Pro is similar to PyCharm Professional while functionality is worse than PyCharm Community

> ..

**And the winner is.. Jupyter Notebook**

> .. 

**More seriously**

PyCharm while debugging (with Sublime keymap) and Sublime for everything rest. Atom and VStudio are on the horizon, but Electron apps are currently not comparable to pure C/C++ engine.

--- 

**RAM usage while testing editors (single python file opened)**

* Sublime Text - 15-40 MB 
* PyCharm - 400-600 MB
* Wing IDE - 40-100 MB
