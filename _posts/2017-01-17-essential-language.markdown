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
