---
layout: post
title:  "Pattern matching"
date:   2014-06-18 12:22:40
categories: elixir
---

Coming from a java background, I have been trying out different languages lately like Clojure, Haskell, Erlang, Elixir etc. I started programming more in functional style and it took some time for me to retrain my OO brain to think functionally.
<ul>
<li> - Now I dont feel bad seeing a class with a lot of smaller but well defined static (pure) functions in the code base
<li> - I don't prefer modeling behaviours than data
<li> - I prefer composition over inheritance
<li> - I started prefering bottom up style of development 
<li> - TDD makes more sense to me than before
<li> - Immutability everywhere!
<li> - I keep my eye open to spot an opportunity to do lazy evaluations & make use of streams
<li> - Alergic to side effects 
<li> - Prefer flow based programming
<li> - Functions are my first class citizens
</ul>
... and the list goes on

Having listed down the changes that i have developed in my thinking towards programming, there is one specific change that i developed which i wanted to bring out in this blog which is "alergic to if/else conditions"

Every programmer would have used if's and else's in their code, but have we ever wondered if we can avoid it? 

The master OO programers try to avoid if conditions by using interfaces and dynamic dispatching, which is fine... but you cant just keep creating classes and objects for every simple condition that you want to check, will you?

I don't say if/else should be avoided, but i found better ways to get past them that makes your code look neater and more explicit compared to the if/else clauses.

Suppose that you are processing a data structure and perform different action based on the elements in the data structure, the typical javascript code would look like this:
{% highlight javascript %}

function action1(data){
  // perform action 1
}

function action2(data) {
 // perform action 2
}

function action3(data) {
 // perform action 3
}

function process(data) {
    if (data.key1) {
        action1(data)
    }
    else if (data.key2){
        action2(data)
    }
    else {
    	action3(data)
    }
}
process(var data = {'key1' : 1});

{% endhighlight %}


what if you have 5 types of actions that you need to do based on the data? you will end up using a lot of if else conditions! I know that you can break them down into classes and make use of dynamic dispatch to get past it, but i wont do that often especially when i am working in a functional paradigm.

This is where pattern matching comes to the rescue, let's see how the example above can be written in Elixir.

{% highlight elixir %}

def action(%{key1: value} = data) do
	# perform action 1
end

def action(%{key2: value} = data) do
    # perform action 2
end

# and so on...


action(%{key1: 1})
{% endhighlight %}

Did you notice that you have completely removed the if/else conditions but instead wrote a self explanatory functions, which when called resolves dynamically to the right implementation by pattern matching the argument against the function declaration.

This approach not only reduces the code footprint but makes your code very explicit and easily understandable, you need not have to break your head coming up with different names for differnt actions though they do the same action (with minor differences)

I wish we get pattern matching in all languages in future and we prefer more and more declarative style of programming over procedural style!





