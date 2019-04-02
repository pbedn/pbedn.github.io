+++
title = "Method overloading in Python with example from Java"
date = "2019-04-02"
draft = false
tags = ['python', 'java']
+++

Coming from for Java world we could ask what the best way for method overloading in Python language is. There are many ways to achieve that; some are even considered an anti-patterns. Let's through a few of them and choose the best.
  
<!--more-->

We can think of three cases when dealing with method overloading.

1. Different data type of parameters with the same number
1. Different order of parameters
1. Different number of parameters of the same type

Let's take a simple example from Java which works out of the box, and then convert it to Python. 

```java
// JAVA
public class Main {
    public static void main(String[] args) {
        printScore("Paul", 100);
        printScore(100, "Paul");
        printScore(50);
        printScore(10, 20, 30, 40);
        printScore();
    }
    // case 1
    private static void printScore(String playerName, int score) {
        System.out.println("Player " + playerName + " scored " + score + " points");
    }
    // case 2
    private static void printScore(int score, String playerName) {
        System.out.println("Player " + playerName + " scored " + score + " points");
    }
    // case 3
    private static void printScore(int score) {
        System.out.println("Unnamed player scored " + score + " points");
    }
    // case 3
    private static int printScore(int...scores) {
        int sum = 0;
        for (int i=0; i < scores.length; i++) {
            sum += scores[i];
        }
        System.out.println("Unnamed player scored " + sum + " points");
    }
    // case 3
    private static void printScore() {
        System.out.println("No player no score");
    }
}
```

```python
# PYTHON 3.6+
class Main:
    def __init__(self):
        self.print_score("Paul", 100)
        self.print_score(100, "Paul")
        self.print_score(50)
        self.print_score(10, 20, 30, 40)
        self.print_score()

    def print_score(self, player_name, score):
        print(f"Player {player_name} scored {score} points")

    def print_score(self, score, player_name):
        print(f"Player {player_name} scored {score} points")

    def print_score(self, score):
        print(f"Unnamed player scored {score} points")

    def print_score(self, *scores):
        my_sum = sum(arg for arg in scores)
        print(f"Unnamed player scored {my_sum} points")

    def print_score(self, score):
        print("No player no score")
```

Obviously, in Python we get **_TypeError_** while trying to run this piece of code.

The easiest way to fix it is to change names of methods, and as a result, we get multiple functions with similar functionality.
However not a very elegant solution.

```python
class Main:
    def __init__(self):
        self.print_score_1("Paul", 100)
        self.print_score_2(100, "Paul")
        self.print_score_3(50)
        self.print_score_4(10, 20, 30, 40)
        self.print_score_5()

    def print_score_1(self, player_name, score):
        print(f"Player {player_name} scored {score} points")

    def print_score_2(self, score, player_name):
        print(f"Player {player_name} scored {score} points")

    def print_score_3(self, score):
        print(f"Unnamed player scored {score} points")

    def print_score_4(self, *scores):
        my_sum = sum(score for score in scores)
        print(f"Unnamed player scored {my_sum} points")

    def print_score_5(self):
        print("No player no score")
```

The second option would be to have only one method which checks for number and type of parameters using if-else clause.
That is usually considered an anti-pattern in Python. So think twice about your design when coding this way.

```python
class Main:
    def __init__(self):
        self.print_score("Paul", 100)
        self.print_score(100, "Paul")
        self.print_score(50)
        self.print_score(10, 20, 30, 40)
        self.print_score()
        
    def print_score(self, *args):
        if not args:
            print("No player no score")
        elif len(args) == 1:
            score = args[0]
            print(f"Unnamed player scored {score} points")
        else:
            if isinstance(args[0], str) and isinstance(args[1], int):
                player_name, score = args
                print(f"Player {player_name} scored {score} points")
            elif isinstance(args[1], str) and isinstance(args[0], int):
                score, player_name = args
                print(f"Player {player_name} scored {score} points")
            else:
                # will raise TypeError in case of wrong argument type
                my_sum = sum(score for score in args)
                print(f"Unnamed player scored {my_sum} points")
```

Our third option that does not exist in Java is a feature called **_keyword (named) arguments_**. This way we can achieve 
much better readiness and give ourselves +1 star for being more pythonic ;) However, there are cases when using keyword arguments 
does not help, for example when overloading constructors - see [this blog post][overloading_constructors] about a particular problem with API and proposed solution with factory pattern and @classmethod.

```python
class Main:
    def __init__(self):
        self.print_score(player_name="Paul", score=100)
        self.print_score(score=100, player_name="Paul")
        self.print_score(50)
        self.print_score(10, 20, 30, 40)
        self.print_score()
        
    def print_score(self, *scores, player_name=None, score=None):
        if player_name and score:
            print(f"Player {player_name} scored {score} points")
        elif scores:
            my_sum = sum(score for score in scores)
            print(f"Unnamed player scored {my_sum} points")
        else:
            print("No player no score")
```

Finally, we can introduce newer kid on the block - **[singledispatch][single_dispatch_python]** decorator.
Citing from Python documentation single dispatch is 

> *a form of generic function dispatch where the implementation is chosen based on the type of a single argument.* 

> _but the dispatch happens on the type of the **first** argument_

Can we use it in our example? Yes and no. Why? After all, methods are functions (well, sort of).
A method is a piece of code that is called by a name like a function but is associated with an object, and

* is implicitly passed the object on which it was called
* is able to operate on data that is contained within the class

If we would like to use single dispatch generic functions we need to change them into *bound methods* ([read more here][lukasz_langa_notes]).

Our example modified with single dispatch shows that it is not possible to catch all cases. I had to remove two of them. One with arguments switched, 
and one without any arguments (single dispatch requires at least one positional). 


```python
class Main:
    def __init__(self):
        self.print_score = singledispatch(self.print_score)
        self.print_score.register(str, self.print_score_name)
        self.print_score.register(int, self.print_score_no_name)
        self.print_score("Paul", 100)
        self.print_score(50)
        self.print_score(10, 20, 30, 40)

    def print_score(self, *args):
        raise NotImplementedError("Unsupported type")

    def print_score_name(self, player_name, score):
        print(f"Player {player_name} scored {score} points")

    def print_score_no_name(self, *scores):
        my_sum = sum(score for score in scores)
        print(f"Unnamed player scored {my_sum} points")
```

All in all, this was a fun exercise. I wonder what options I missed here.
One might be multiple dispatch which I might explore next time.

Additional notes:

* General JVMs (Java Virtual Machines) only use single dispatch
* Python has @singledispatch decorator since 3.4, for previous versions install *singledispatch* from pypi (2.6+, 3.2+). 

---

References:

* [Overloading constructors in Python - blog post][overloading_constructors]
* [Single Dispatch - Python Docs][single_dispatch_python]
* [Difference between method and function - Stackoverflow][so_difference_method_function]
* [Single Dispatch - Lukasz Langa notes][lukasz_langa_notes]


[overloading_constructors]: https://stavshamir.github.io/python/2018/05/26/overloading-constructors-in-python.html
[single_dispatch_python]: https://docs.python.org/3/library/functools.html#functools.singledispatch
[so_difference_method_function]: https://stackoverflow.com/questions/155609/whats-the-difference-between-a-method-and-a-function
[lukasz_langa_notes]: http://lukasz.langa.pl/8/single-dispatch-generic-functions/
