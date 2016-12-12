---
layout: post
title:  SBCL & Swank in docker
date:   2015-04-04
categories: devops
---

Recently I started reading the wonderful book "on lisp" by Paul Graham. As I wanted to work on the examples in the book, I created a docker container with SBCL & Swank installed. This allows me to start swank quickly and connect my emacs to it. <br/><br/>

I have published the container on docker hub, you could pull it by running
{% highlight bash %}
docker pull yehohanan7/clisp
{% endhighlight %}
<br/><br/>

However, you could also checkout the Dockerfile from https://github.com/yehohanan7/clisp.git and build it yourself, this will checkout sbcl & slime and build it from source. <br/><br/>


Once you have built/pulled the docker image, you can start swank easily by running the below command

{% highlight bash %}
docker run -i -t -p 4005:4005 yehohanan7/clisp swank
{% endhighlight %}
<br/><br/>


