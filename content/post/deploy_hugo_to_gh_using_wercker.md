+++
tags = ["hugo"]
draft = false
date = "2017-03-17T08:25:40+01:00"
title = "Deploy Hugo to Github Pages using Wercker"

+++

The easiest solution how to publish with Hugo to Github Pages is to have two separate repositories, one with blog source files and second with html files. Simple.

We can push files from both repositories manually or can do better. Push just source content and leave the rest for automatic deployment.

I did not know about Wercker until read [automated deployment tutorial](https://gohugo.io/tutorials/automated-deployments/) on Hugo site. And with hint from [Skarlso post](http://skarlso.github.io/2016/02/10/hugo-autodeploy-with-wercker/), that process became so easy.

Steps:

1. Create new application on Wercker with Hugo source repository. Leave the recommended settings.
2. In *Workflows* add new *Pipeline* with both names set to *deploy*.
3. Create new *Personal access token* on Github, and copy its value to Wercker *Environment*, with key name: *GIT_TOKEN*.
4. Add *wercker.yml* to your Hugo source repository. Just change your Github name.

    ```
    box: golang
    build:
        steps:
            - arjen/hugo-build:
                version: "0.19"
    deploy:
        steps:
            - install-packages:
                packages: git ssh-client
            - leipert/git-push:
                gh_oauth: $GIT_TOKEN
                repo: <name>/<name>.github.io
                branch: master
                basedir: public

    ```

5. Push and enjoy.

Note that in *arjen/hugo-build* I do not use *theme* option, I have all layouts and static files in my [hugo-blog](https://github.com/pbedn/hugo-blog). For more info go to [arjen docs](https://github.com/ArjenSchwarz/wercker-step-hugo-build).
