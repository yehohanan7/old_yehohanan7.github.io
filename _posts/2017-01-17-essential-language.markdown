---
layout: post
title:  "Essential Language"
date:   2017-01-17 12:22:41
categories: golang
comments: true
---
<h3>Essential Language</h3>

When everything is going well and one fine morning Mr Bob shows up with full of excitement, guess what? He learnt Scala and while learning Scala he also got acquainted with functional programming, now he thinks his brain is wired in a completely new way! he wants to use Scala on the project!

Although i felt happy that Bob learned functional programming, i was taken aback when he started <b>rewriting</b> java code in Scala. what is going on in his mind? is Scala the only way to write functional programming? is Scala even necessary to write functional code?

Well, I am not against Scala or any language for that matter. Ever since I read SICP (structure and interpretation of computer programs), I fell in love with elegant programming techniques and simplicity and I always explore different computational/programming models. I did a lot of common lisp & Clojure on my hobby projects and loved LISP and the ideas of lisp which is unique and original, thanks to John McCarthy!


I also liked ideas of Haskell which is striving hard to be the purest functional language! I support Haskell to see how far we could get with building higher and higher levels of abstractions.
Ok now you will agree that I love functional programming, I embrace every ideas proposed by functional programming, but I hate the idea that you could do functional programming only in languages like Scala, Haskell et al.

I do agree it makes it a bit easy syntactically, but languages like Scala encourage people to build the second type of software as quoted here by Hoar “There are two ways of constructing a software design. One way is to make it so simple that there are obviously no deficiencies. And the other way is to make it so complicated that there are no obvious deficiencies.”

There are few people who work on complex modeling problems who might need a lot of support from the language, I am not fortunate enough to work on such domains yet. But majority of the developers (be it Fullstack, half stack, backend engineer, devops engineer et al.) work on projects that involves interacting heavily with real world — move data between micro services, store data, orchestrate between multiple services, react to events, send emails, measure activities, automate everything that could be automated, monitor systems, etc.
Do you see a common patterns in the kind of work people do?

<ul>
<li>side effect
<li>parallel
<li>efficiency
<li>maintainability/readability
<li>fast & continuous delivery
</ul>


If you agree that your application belongs to the category mentioned above, where majority of your code has side effects, trying to use IO monads and side effect free functions will lead to a mess!
Let’s see what are the benefits you would get from a functional language like Mr Bob would propose:

<h4>Pure functions</h4>

I think this point can be easily ruled out because you can write pure functions in any language


<h4>Concurrency</h4>

I agree that languages like Scala helps you write concurrent code, the moment you separate pure functions from impure ones, it becomes even more easier to write highly concurrent code. but there are languages like nodejs, go, erlang (actor model) which are designed keeping this as the primary goal of the language. Although scala copied the actor model from Erlang, it still uses native threads and thread pool which is not as efficient as Erlang’s or Golangs concurrency model.


<h4>Readability & Maintainability</h4>

This is the topic Mr Bob would use it for his advantage, because it’s a purely subjective argument!
A language should not decide if you could write readable code or not, a smart developer would write readable & maintainable code in any language. Some people argue that less code is always readable code and try to literally compress the code using all syntactic sugars available at their disposal, well then they should write their programs in brain fuck language! sometimes, you write more code to make it readable, not always less - you should be pragmatic enough to take that call.

In my opinion, the more options a language gives you, the more subjective arguments you will have with fellow developers. It is a common trend for developers to argue and fight over who’s syntax is better - if statements vs filter calls, if/else vs optionals & maybe monads, loops vs foreach, map, fold, foldRight, foldLeft, foldTop, foldBottom whatever.
I think google learnt a very valuable lesson, they realized the above mentioned problems and decided to design a language with very clear goals like simplicity, readability, maintainability and concurrency, as a result golang was born!

Let’s look at the design goals behind Scala - Well, it uses JVM because of the huge existing community, copied actor model from Erlang but uses native OS threads(not a great idea), copied most ideas from Haskell and kept the doors open for java developers to actually write java code if they like to in Scala files! And there are tons of options for you to do any simple task which hindering progress. you might argue that the time spent thinking about options is not a lot, but there is a compounding effect to it - As the saying goes “Elephants don’t bite, Ants do”, its always the small things that hider progress not the big ones.

Some people like this idea of freedom given by Scala, but i see that as pure noise! Say for example I am pairing with another developer and I want to loop through a collection of objects and perform network calls for each element. If I used golang then I just have to focus on the problem at hand and will start writing the code instantly because there’s just one way to loop. But imagine I used Scala, i start off with a for loop and my fellow developer is an ardent functional programming geek and he wants to use map/filter to do the same thing, once we did that then we argue on handling exceptions either the java way or scala’s Try monad way. After settling on that argument, you end up making the code written fluent by using various syntactic sugars offered by Scala which I believe is a lot and hence you waste time arguing on subjective preferences. Alright, so eventually what benefit has Scala brought other than making you think a lot about how to express the solution rather than focusing on the problem at hand and making progress?

You can write elegant programs in any programming language if you are a good programmer. You can get all the benefits of functional programming by just using java or golang if only you can think in abstractions and build bottom up with small pieces of abstractions. Always borrow the “values” from other languages and paradigms but not their syntaxes.
All choices you make in choosing languages or architectural decisions should be based on simplicity and pure essentialism! Be an essentialist, if you are working for a business then choose the technologies that’s essential for the business and not to have just fun trying something new. Being an essentialist is more fun than trying a new language!


In conclusion, when you choose a language for a specific problem then ask yourself if there is a measurable benefit for the business by choosing the new language, if there isn't a clear yes then it’s a very clear NO!


<b>“Discern the vital few from the trivial many”</b>
