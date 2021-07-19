+++
title = "Simple game menu in Python and Godot"
date = "2020-07-17"
draft = false
tags = ['godot', 'python', 'pyglet']
+++

Recently I have started to dive into game programming a bit more. Coming from Python with Pyglet, 
my current choice is Godot game engine. I created a simple text-based game menu in both to see what is easier for someone without any prior experience.

<!--more-->

#### Python - Pyglet - Gooey

[Glooey][glooey] is an object-oriented GUI library, based on Pyglet (cross-platform windowing and multimedia library).

![Gooey](../img/glooey-menu.gif)

[Checkout Github repository for Glooey game menu][glooey-game-hud-example]

#### GDScript - Godot

[Godot][godot] is an open-source game engine with 2D and 3D capabilities. 
The officially supported languages are GDScript (Python-like), Visual Scripting, C#, and C++

![Godot](../img/godot-menu.gif)

[Checkout Github repository for Godot game menu][godot-game-hud-example]


#### Summary

No surprise here, Godot is much simpler, and enjoyable, though Glooey is a very solid library.
If you would like to craft a game menu specifically using Python, I can recommend Glooey for that.

Godot is very flexible because it is not drag and drop, and you have to use scripting for various actions. One thing I had to fight through was font scaling, I could not do it simply as
in Glooey, but instead, use animation and some hacks. Is there a better way?


---

Resources:

* [github: Godot game menu][godot-game-hud-example]
* [github: Python game menu][glooey-game-hud-example]
* [Glooey: An object-oriented GUI library for pyglet][glooey]
* [Godot game engine main site][godot]

[godot-game-hud-example]: https://github.com/pbedn/godot-game-hud-example
[glooey-game-hud-example]: https://github.com/pbedn/glooey-game-hud-example
[glooey]: https://github.com/kxgames/glooey
[godot]: https://godotengine.org/
