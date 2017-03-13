+++
date = "2017-03-13T11:50:42+01:00"
title = "Hello World with Hugo"
draft = false
+++

While trying to make this little space in the web finally working, I set up dozens of different static site generators (Jekyll, Pelican, Nikola, Lektor, Sculpin..). I was even thinking about writing my own for some time. I tried wordpress and blogspot. But finally arrived at [Hugo](https://gohugo.io/) and it make my day much better.
<!--more-->

Why I like it:

* Nice working on Windows (one '.exe' file)
* Good documentation

Some things that could be better:

* Better deployment to Github Pages

--- 

### Quick tips:

After some fight to publish it nicely to Github Pages, I tried to use automate deployment (Wecker tutorial on Hugo website doesn't work out of the box, and my knowledge on CI is not good yet). Finally I settled easiest solution - having two repositories. 

* First repository is 'User Page' - 'pbedn.github.io'. Please refer to [pages docs](https://help.github.com/articles/user-organization-and-project-pages/) about more info.
* Second is with source content of hugo - 'hugo-blog'

How to maintain pushing to two repositories? I use GitKraken, so it is just few clicks. Additonaly I am all the time inside 'hugo-blog' directory.

1. When I create new content I run command
    ```
    hugo serve -w -D
    ```

2. When I want to push to github, firstly I create html files
    ```
    hugo -d ../pbedn.github.io
    ```
    And use GitKraken.


#### What if after running hugo serve I do not see anytinng in the *public* directory?

Sometimes it helps to run hugo with '-v', it shows more verbose output. My solution was to copy theme files inside my main folder (sometimes you just have to copy static files).