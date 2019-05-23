+++
title = "Summary: Python Enhancement Proposal (PEP) #1"
date = "2019-05-23"
draft = false
tags = ['python']
+++

PEP (Python Enhancement Proposal) are describing new features, processes or other guidelines and information.
They are written primarily for core CPython or other implementation developers. 
But actually, they are fun and interesting read for a regular developer as well. It is because the authors are really polishing
the documents (standards are rigorous) and in addition to documentation, we can get to know proposed or already implemented features.
Anyway, fun to read. One disadvantage, PEPs are sometimes long.

<!--more-->

The first post in my new mini-series will be a brief introduction to what exactly are PEPs.
And the best way to start is to actually review and try to summarize the first PEP ever with number 1.
It belongs to a group called Meta-PEPs (PEPs about PEPs or Processes). 
Well, there exists PEP 0, but it is just an index of everything.

Everyone can write and propose a new PEP. But if an author is not a core developer, then the author needs to find one
called 'the sponsor', who will guide on logistics of the whole process. Currently, Python's Steering Council decides if single
PEP will be accepted or rejected. Previously it was BDFL ("Benevolent Dictator for Life") or BDFL-delegate.

**PEPs Index by Category:**

* Meta-PEPs (PEPs about PEPs or Processes)
* Other Informational PEPs
* Provisional PEPs (provisionally accepted; interface may still change)
* Accepted PEPs (accepted; may not be implemented yet)
* Open PEPs (under consideration)
* Finished PEPs (done, with a stable interface)
* Historical Meta-PEPs and Informational PEPs
* Deferred PEPs (postponed pending further research or updates)
* Abandoned, Withdrawn, and Rejected PEPs

The category index describes nicely little information about different PEP types. With the most interesting categories are:
*Accepted*: because they might not be implemented yet, but we could start diving into them. And *Open*: to catch promising ideas.

In next parts I will look into two proposals with 'Accepted' status that will be part of Python 3.8:

* 570 - Python Positional-Only Parameters
* 572 - Assignment Expressions

 

---

References:

* [Link to main PEPs site][link_to_peps]


[link_to_peps]: https://www.python.org/dev/peps/
