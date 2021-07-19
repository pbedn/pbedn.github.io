+++
title = "Python evolution with Luciano Ramalho - summary of podcast"
date = "2021-07-19"
draft = false
tags = ['python']
+++

In one of the recent episode of [Python podcast][podcast], Toby Macey was interviewing Luciano Ramalho. I have a lot of respect for this developer as for me it is one of the last true "Pythonista's" left in the world. During one hour he talks about his Python beginnings, writing "Fluent Python" book, and his view about new Python features. I recommend listening to the whole podcast, but here is a summary of his opinions about Python evolution. 

<!--more-->

### Python evolution - Luciano comments

#### Major features

* Asyncio (3.4)
* Type hints (3.5) - Typing was introduced when Guido was working in Dropbox (other big companies like Facebook and Google needed that as well).
  * Typing is enjoyable only in Python 3.9, after introduction type hinting generics, earlier good addition was typing protocol in 3.8.
  * Type system (static type) evangelist should not push the idea that everyone should use it, as it introduces unnecessary complexity, especially in digital humanities, scientific, data science areas.
  * Abstract of [research paper][paper]: This paper argues that we should seek the golden middle way between dynamically and statically typed languages.

#### Minor features:

* Matrix Multiplication Operator (3.5) - good addition as it is completely harmless for people who do not need it, great for people who need it (scientific community).
* PEG Parser (3.9) - while the PEG style of a parser is great computer science development of the 21st century, we should worry because as it is expressive it is very complex, and we will have complex features in Python.
  * [PEP 3099][pep3099] - Things that will Not change in Python 3000 - this PEP says that the parser should not be more complex than LL(1).
  * Immediately after PEG is accepted, Python will have pattern matching.
* Structural Pattern Matching (3.10) - such major change should be introduced very carefully and slowly, simplified first so it could be easily understood (and not by writing five PEPs, because in one it cannot be explained).
* Assignment expressions, walrus operator (3.8) - it is a hard feature to explain for newcomers to programming and Python. Not worth introducing.

#### Other thoughts

* The community should recognise that the ease of learning Python is one of its strongest points. 
* Python is popular because data science and machine learning happened at the right moment. Python was high level, approachable and had good tools and libraries.
* Characteristic of good language feature: 
  * enhances the language for the advanced users so they can provide facilities for other users
  * does not intrude
* Core developers should spend time and prioritise things like:
  * support for mobile applications (biggest weakness of Python)
  * support for multicore applications (a lot of people moving to Go)

### My opinion

Walrus operator - Guido decided to leave the BDFL role mainly because of community split and strong discussions after the introduction of this feature. I respect Guido work but everybody gets old one day.

I am a self-taught programmer. My first language was Python, and for many years I was devoted to this language (involved in the local community, listening to podcasts, reading PEPs and Python dev mailing lists). And I agree with Luciano that simplicity and ease of use was the strongest point that kept me. After years I think it was a good choice, however currently the trend I am seeing worries me the same as Luciano. 

On my bookshelf, I have C# book because it is a language to go for video game programming. I have the Golang book because it is well thought, backward compatible, simple language for the concurrent web. And Python, it still brings me the paycheck...

---

Resources:

* [How Python's Evolution Impacts Your Fluency With Luciano Ramalho - Python Podcast Episode 296][podcast]
* [Static Typing Where Possible, Dynamic Typing When Needed: The End of the Cold War Between Programming Languages - paper][paper]
* [PEP 3099 - Things that will Not Change in Python 3000][pep3099]

[podcast]: https://www.pythonpodcast.com/luciano-ramalho-python-evolution-episode-296/
[paper]: https://www.researchgate.net/publication/213886116_Static_Typing_Where_Possible_Dynamic_Typing_When_Needed_The_End_of_the_Cold_War_Between_Programming_Languages
[pep3099]: https://www.python.org/dev/peps/pep-3099/

