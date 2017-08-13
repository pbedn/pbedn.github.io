+++
date = "2017-06-21"
title = "How to make automatic deployment with git: 'fetch and checkout' vs 'pull --ff-only'"
tags = ['git']
draft = false
+++

Can git be used as automatic deploment tool? Some say that as for mvp it is enough. Others give quite good explanations how to do it, see [gitolite.com/deploy](http://gitolite.com/deploy.html).

<!--more-->

My first naive solution was to use in deployment script for production, commands:

```bash
$ git checkout .
$ git pull --ff-only
```

It works if production repository is clean. But sometimes there are problems with merging, and unless you catch the error, remote deploy script will just give you the message "*fatal: Not possible to fast-forward, aborting.*", leaving the repository in bad shape.. So what is the proper solution?

Let say that in production environment "*git log --oneline*" gives me:

```bash
114dbc5 Some changes
2fe6adb Some changes
e38c033 Added broken file
6f6ba61 Changed Readme
ab6d78a Initial commit
```

In development environment I did a "*git rebase -i 6f6ba*", squashing last two commits into one, and "*git push --force*" to repair my master branch. That will give me:

```bash
5e88aca Some changes
e38c033 Added broken file
6f6ba61 Changed Readme
ab6d78a Initial commit
```

My deployment script will run in production and produce output:

```bash
$ git pull --ff-only
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/pbedn/git-playground
 + 114dbc5...5e88aca master       -> origin/master  (forced update)
 * [new tag]         after-rebase -> after-rebase
fatal: Not possible to fast-forward, aborting.
```

That is a problem. Solution is to use:

```bash
$ git fetch --all
$ git checkout --force origin/master
```

Commits in the production are now correct!

```bash
$ git fetch --all
Fetching origin

$ git checkout --force origin/master
Note: checking out 'origin/master'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at 5e88aca... Some changes

$ git log --oneline
5e88aca Some changes
e38c033 Added broken file
6f6ba61 Changed Readme
ab6d78a Initial commit
```

--

*Exercise*

If you like to play with these exact examples you can follow the steps below (note that we are working with HEAD detached, but for the exercise purpose that is ok):

```bash
$ git clone https://github.com/pbedn/git-playground
$ git checkout tags/duplicate
$ git pull --ff-only origin master
$ git checkout tags/after-rebase
$ git fetch --all
$ git checkout --force origin/master
```
