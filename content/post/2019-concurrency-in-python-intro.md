+++
title = "Concurrency in Python - intro"
date = "2019-10-02"
draft = false
tags = ['python', 'concurrency']
+++

The first paragraph from Python documentation about concurrency titled "Concurrent Execution" mentions a few phrases like, 
CPU bound, IO bound, event-driven cooperative multitasking or preemptive multitasking.
In this intro post to concurrency, let's clarify these definitions.

<!--more-->

#### CPU bound vs IO bound

In the simplest form, CPU bound program, have time-consuming, high usage computation task, that is heavy on 
CPU - central processing unit. This is our bottleneck, and by upgrading the computer speed, we can improve the execution of our program.

On the other hand, IO (I/O - input/output) bound means that we are waiting for some subsystem to deliver the data.
For example, hard disk, network, database, or another kind of communication device, process some data, and our program
is waiting for the result. The best way to optimize the improve is to optimize our program.

There are other bottlenecks visible sometimes in the software world, for example, memory bound or cache bound, however 
these are special variants of general I/O bound definition.

#### Preemptive multitasking

Going into something more difficult. Preemption is "act of temporarily interrupting a task being carried out by
a computer system.. with the intention of resuming the task at a later time". [Preemptive in Wikipedia][preemptive].
That means that at any time operating system (OS) can stop our program and switch context to another task. 
Allowing OS to manage threads is the easiest way, however with a disadvantage of interrupting in the wrong moment.
In Python, the threading module uses this technique.


#### Event-driven cooperative multitasking

Cooperative multitasking, on the other hand, never initiates a context switch. Instead, the program must yield control. 
[Cooperative in Wikipedia][cooperative]. Advantage of that is that we always know when the task will be stopped and context switched. However, our program must be modified for that. In Python, asyncio module use this technique.

#### Multiprocessing

Both techniques described above use a single CPU for the program. If we would like to employ the rest of the cores,
then the multiprocessing module in Python allows for that. That way, we can achieve true parallelism, as each program
is run in the separate core.

To summarize all three concurrency types, let's look at a table below [Real Python post about concurrency][real-concurrency]:

| Concurrency Type | Switching Decision | Number of Processors |
|---|---|---|
| Pre-emptive multitasking (threading | OS decides when to switch tasks | 1 |
| Cooperative multitasking (asyncio) | Program decides when to give up control | 1 |
| Multiprocessing (multiprocessing) | Processes run at the same time on different cores | Many |

---


References:

* [Preemptive multitasking - Wikipedia][preemption]
* [Cooperative multitasking][cooperative]
* [Concurrency - Real Python][real-concurrency]

[preemption]: https://en.wikipedia.org/wiki/Preemption_(computing)
[cooperative]: https://en.wikipedia.org/wiki/Cooperative_multitasking
[real-concurrency]: https://realpython.com/python-concurrency
