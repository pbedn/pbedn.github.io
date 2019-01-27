+++
title = "Best Django hosting server"
date = "2019-01-27"
draft = false
tags = ['django', 'ubuntu']
+++

In my spare time, I am creating small Django web application, designed for language schools administration. This month I decided to deploy it to the server finally. Quick research gave me a few possibilities: home setup with raspberry pi (kind of free), pythonanywhere.com (free/paid), heroku.com (free/paid), digitalocean.com (paid), linode.com (paid). I have chosen DigitalOcean, and here are is why.

<!--more-->

Pros and cons: RaspberryPi:

* Good: fairly cheap computer and easy to setup
* Bad: slow SD card makes updates painful
* Ugly: need to set up a good firewall and buy DNS as I have a dynamic IP address

Pros and cons: pythonanywhere.com

* Good: it is free, and I had used it before because Harry Percival recommended it (Testing Goat? ;)
* Bad: site UI/UX, lack of documentation, old python and Django versions available
* Ugly: seems like they stopped developing that service, or most of the things are blocked for free users

Pros and cons: heroku.com

* Good: FreeDyno and excellent app configuration options
* Bad: HobbyDyno is 7$/mo that gives me only 512MB RAM
* Ugly: I give up quickly after app creation, and researching deploy options. Did not recognise Poetry (pyproject.toml) 

Pros and cons: digitalocean.com

* Good: fast Linux machine of choice (1vCPU, 1GB RAM, 25GB SSD), great docs and tutorials
* Bad: no free plans and the cheapest virtual machine is 5$/mo
* Ugly: did not find anything yet

What about linode.com?

To be honest, I did not try this service, but with the similar pricing to digitalocean.com, I have chosen the latter, because most of the tutorials of how to setup ubuntu with Django comes from the Digital community, and initial zero-problem configuration proved that choice.

---

After such marketing, Digital should pay me ;) but well at least you can click my referral link to create an account there.. [Referral Link](https://m.do.co/c/79380110cc20)
