---
layout: post
title:  "Callback vs Reactive"
date:   2014-04-25 12:22:40
categories: erlang
---

<h3><small>Introduction</small></h3>
Both async callbacks and reactive style programming helps us write non-sequential non-imperative code,
but i personally prefer message passing style programming adopted in languages like Erlang due to its simplicity and the scalability of erlang platform itself.
In this blog, i will try and explain how we should program in an asynchronous environment with few examples and also list out the points for which i like to use Erlang/Elixir for the same over NodeJS.

Consider an api written in javascript which uses callback mechanism to search for a text in various social networking
sites using specific services for each site. The API aggregates all the responses and returns a combined result to the client.

{% highlight javascript %}
  ...
  function search(text){
      var results = [];
      facebook.search(text, function(fbResults){
          results = results.concat(fbResults);
      });
      twitter.search(text, function(twitterResults){
        results = results.concat(twitterResults);
      });
      linkedIn.search(text, function(linkedInResults){
          results = results.concat(linkedInResults);
      });
  }
  ...
{% endhighlight %}

in the above code, we try to use NodeJS' non blocking IO to make requests to various sites in parallel.

Note: Though NodeJS is single threaded, it does internally maintain a dedicated thread (pool) to poll for IO events and populate the event channel accordingly so that
NodeJS' event loop can pick it up and call the necessary functions.

One problem that emerges out here is how do we return the consolidated results to the client? yes, we could take a callback in the search function and call it when we have received responses
from all 3 api calls, but that demands us to maintain a state (a counter) to decide when to call back the client/caller.

Example:

{% highlight javascript %}
  ...
  function search(text, callback){
      var results = [];
      var count = 0; //state
      facebook.search(text, function(fbResults){
          results = results.concat(fbResults);
          ...
          // update state and call back the function if the incremented count is 3
      });
      twitter.search(text, function(twitterResults){
        results = results.concat(twitterResults);
        ...
        // update state and call back the function if the incremented count is 3
      });
      linkedIn.search(text, function(linkedInResults){
          results = results.concat(linkedInResults);
          ...
          // update state and call back the function if the incremented count is 3
      });
  }
  ...
{% endhighlight %}

if you don't want to maintain the state, you could also try to serialise the API calls like below.

{% highlight javascript %}
  ...
  function search(text, callback){
      var results = [];
      facebook.search(text, function(fbResults){
          results = results.concat(fbResults);
          twitter.search(text, function(twitterResults){
            results = results.concat(twitterResults);
            linkedIn.search(text, function(linkedInResults){
                results = results.concat(linkedInResults);
                callback(results);
            });
          });  
      });
  }
  ...
{% endhighlight %}

but this nested callback approach hurts my brain to understand what's happening, i would never write such a code!

so what are we left out with? should we use promises? um yes, but that again doesn't add any value to the code, your code will still be nested like the callback approach with some "then"s and "resolve"s added.

<h3><small>Push the data</small></h3>

How about pushing the results to the client as and when we get them from various APIs without collating it? but this demands your entire architecture to be reactive in nature!

{% highlight javascript %}
  ...
  function search(text, callback){
      facebook.search(text, function(fbResults){
          callback(fbResults);
      });
      twitter.search(text, function(twitterResults){
        callback(twitterResults);
      });  
      linkedIn.search(text, function(linkedInResults){
          callback(linkedInResults);
      });
  }
  ...
{% endhighlight %}

The above code pushes the results to the client in parts as we receive results from facebook, twitter and linkedIn services, the code looks very neat and explicit now!
what if the client wants to send a consolidated view of all results to the browser for display? then the client also should push the data to the browser using websockets so that the view (DOM) is built incrementally.

{% highlight javascript %}
...
 search('foo bar', function(results){
     socket.send(results); //pushes data
 })
...
{% endhighlight %}


Look at the way your code changed when you made a small paradigm shift of pushing data to client rather than allowing the client to pull the data.
You can now write all layers of your code in a very reactive manner by not maintaining any intermediate state, but just a set of stateless functions in one layer which transforms data and pushes it to the layer beneath.

Now the data flows from the source (fb, twitter, linkedin) to the UI very smoothly, refer to "flow based programming" to know more about this style of programming paradigm

<h3><small>Erlang and Elixir</small></h3>

Why Erlang and Elxir now?
I feel erlang and elixir (both running on the erlang virtual machine or beam) are designed to support such flow based architecture using message passing, it scales better than NodeJS!
<br>NodeJS is single threaded, but EVM is multi threaded and it has it's own scheduler to make the most of your multi core.
<br>we could spawn multiple nodeJS threads to scale horizontally, but Erlang processes are very lightweight than NodeJS because Erlang processes are not native OS threads like NodeJS, which means faster "context switching"!

Apart from concurrency model, other features of erlang that makes me choose it over NodeJs are:
<ul>
<li>immutability
<li>pattern matching (i hate if/elseif/else)
<li>Message passing over callbacks
<li>Location transparency of the processes
<li>Share Nothing philosophy
<li>Fault tolerance strategy for each process
<li>Battle tested OTP
</ul>

In conclusion: I still like Javascript for building simple reactive applications, but if you want to build a really huge enterprise application in a reactive way, I would recommend Erlang Virtual Machine.
