---
layout: post
title:  "Concurrency models"
date:   2014-03-31 12:22:40
categories: erlang
---

<h3><small>Locking and blocking </small></h3>
This is a very simple approach and the most familiar one, if you are a java developer i am sure you would have used it. remember synchronized keyword?

Synchroized keywords are like guards to a shared memory, which serializes access to the shared memory by blocking all threads when an active thread is using the shared memory block. the blocking is done using locks, mutexes, semaphores, etc.

The problem with this lock based concurrency is that it wastes cpu power especially the multicore ones just because a single thread has locked on a particular shared memory! 


<h3><small>Software Transaction memory (STM)</small></h3>

Languages like Clojure has adapted STM, which is a very interesting approach where all threads can access the shared memory concurrently, but each thread will get a local copy of the shared data so they can modify it without disturbing other threads.
once a particular thread has completed modifying the data, it can go ahead and commit the changes back to the shared memory (remember DB transactions?), provided no other thread has committed any new changes since it last read during the begining of the transaction. if modified, the current thread can retry the whole transaction once again. read about MVCC to understand more about how STM will track changes to the data.

This doesn't mean that locks are totally removed in STM! STM still needs lock during the commit stage, but the only difference is that the period for which the thread holds the lock is very minimal and not in your control to misuse, its the platform which handles it safely for you... same like garbage collection which you dont control any more.

The bad part of STM is the retry process, where you need to be aware about the consequences of retries and if you want to do IO within a transaction then you should be very careful about it.

I should mention that clojure community has done some really cool concurrency abstraction using STM, futures, promises etc 

<h3><small>Actor model/Async message passing</small></h3>

This is my favourite approach compared to other models and the simplest one too. with this model you will see hundreds and millions of actors/processes (not threads) running with its own private memory, problemo solved! 
Yes, its very simple approach - "dont share data" but rather pass data if an actor needs it. this approach demands you to model your problem differently as actors and messages, its not object oriented but i feel its better than OO if scaling is your priority. 

Erlang is one language which adapts this model very well though scala & akka follows erlang on the JVM. i prefer erlang's approach because erlang has it's own scheduler to schedule the processes/actors which is very powerful, the scheduler allocated time slice for each actor is at the instruction(s) level and not message level like akka. 
Akka uses the same actor model but it relies on OS thread scheduling (as it uses java thread pools internally). which means, when a message is sent to an actor, it holds the thread till it completes handling the message! which is not bad, but when you compare with erlang i am sure it makes a difference if the load on your application increases massively.