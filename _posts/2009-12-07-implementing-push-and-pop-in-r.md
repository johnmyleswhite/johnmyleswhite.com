---
title: Implementing Push and Pop in R
author: John Myles White
layout: post
permalink: /notebook/2009/12/07/implementing-push-and-pop-in-r/
categories:
  - Statistics
---

Having grown up with Perl, there are two functions that I desperately miss while programming in R: `push` and `pop`. Continually writing

{% highlight r%}
vector <- c(vector, new.entry)
{% endhighlight %}

tries my patience, while writing

{% highlight r%}
vector <- rep(NA, inscrutable.constant)
vector[inscrutable.index] <- new.entry
{% endhighlight %}

makes me feel like I'm programming in C, rather than a higher-level programming language. That said, here's a simplistic hack to provide something like an implementation of `push` and `pop` in R:

{% highlight r%}
push <- function(vector.name, item)
{
  eval.parent(parse(text = paste(vector.name, ' <- c(', vector.name, ', ', item, ')',
                                 sep = '')),
              n = 1)
}

pop <- function(vector.name)
{
  eval.parent(parse(text = paste(vector.name, ' <- ', 
                                 vector.name, '[-length(', vector.name, ')]',
                                 sep = '')),
              n = 1)
}
{% endhighlight %}

Both of these functions are more than a little ugly, because you have to pass in a string that names the vector you want to change, rather than providing its name as a bareword. Even worse, this version of `pop` doesn't let you get the value of the item you pop off of the stack, because I'm not sure how to introduce a temporary variable into the parent's environment without occasionally clobbering the value of an existing variable. If I knew more about scoping and lazy evaluation in R, I think I could implement these two functions as pseudo-macros and solve both concerns. If you know how to do this, please do let me know.
