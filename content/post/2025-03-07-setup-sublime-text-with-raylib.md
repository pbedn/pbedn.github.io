+++
title = "How to setup Sublime Text with Raylib on Windows"
date = "2025-03-07"
draft = false
tags = ['sublime-text', 'raylib']
+++

The official wiki seems to not work nicely describing Sublime setup. Here are some steps to get similar experience to default installation with Notepad++ when working with Sublime Text, and run Raylib examples without issues.

<!--more-->
Tested with Raylib 5.5 and Sublime Text 3 on Windows 10.

1. Download raylib from itch.io and install to default path "C:\raylib".
1. Ensure "C:\raylib\w64devkit\bin" is added to environemtal variable path.
1. Add folder "C:\raylib\raylib" to Sublime project.
1. Create new build system in Sublime named "Raylib.sublime-build" with following content:
    ```
    {
        "shell_cmd": "gcc.exe -o ${file_base_name} ${file} -LC:/raylib/raylib/lib -lraylib -lm -lpthread -lgdi32 -lopengl32 -lwinmm -Iexternal -DPLATFORM_DESKTOP",
        "file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
        "working_dir": "${file_path}",
        "selector": "source.c",
        "shell": false,
        "variants": [
            {
                "name": "Run",
                "shell_cmd": 
                    "gcc.exe -o ${file_base_name} ${file} -LC:/raylib/raylib/lib -lraylib -lm -lpthread -lgdi32 -lopengl32 -lwinmm -Iexternal -DPLATFORM_DESKTOP && ${file_base_name}.exe"
            }
        ]
    }
    ```
1. Open some example file, press Ctrl+Shift+B and choose "Raylib - Run".
1. Enjoy.
