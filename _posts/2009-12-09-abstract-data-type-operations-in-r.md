---
title: Abstract Data Type Operations in R
author: John Myles White
layout: post
permalink: /notebook/2009/12/09/abstract-data-type-operations-in-r/
categories:
  - Statistics
---

This morning, I got a chance to read enough of the [R Language Definition](http://cran.r-project.org/doc/manuals/R-lang.pdf) to finish my implementations of `push` and `pop`. While I was at it, I also wrote implementations of `unshift`, `shift`, `queue` and `dequeue`. Here they are:

{% highlight r %}
push <- function(vector, item)
{
  vector.lvalue.symbol <- substitute(vector)
  new.expression <- paste(vector.lvalue.symbol,
                          ' <- c(', vector.lvalue.symbol, ', ', item, ')',
                          sep = '')
  eval(parse(text = new.expression),
       sys.frame(sys.parent()))
}

pop <- function(vector)
{
  vector.lvalue.symbol <- substitute(vector)
  temp.env <- new.env()
  last.element <- eval(parse(text = paste(vector.lvalue.symbol,
                                          '[length(', vector.lvalue.symbol, ')]',
                                          sep = '')),
                       sys.frame(sys.parent()))
  assign('tmp', last.element, envir = temp.env)
  eval(parse(text = paste(vector.lvalue.symbol,
                          ' <- ', vector.lvalue.symbol,
                          '[-length(', vector.lvalue.symbol, ')]',
                          sep = '')),
       sys.frame(sys.parent()))
  return(get('tmp', envir = temp.env))
}

unshift <- function(vector, item)
{
  vector.lvalue.symbol <- substitute(vector)
  new.expression <- paste(vector.lvalue.symbol,
                          ' <- c(', item, ', ', vector.lvalue.symbol, ')',
                          sep = '')
  eval(parse(text = new.expression), sys.frame(sys.parent()))
}

shift <- function(vector)
{
  vector.lvalue.symbol <- substitute(vector)
  temp.env <- new.env()
  last.element <- eval(parse(text = paste(vector.lvalue.symbol,
                                          '[1]',
                                          sep = '')),
                       sys.frame(sys.parent()))
  assign('tmp', last.element, envir = temp.env)
  eval(parse(text = paste(vector.lvalue.symbol,
                          ' <- ', vector.lvalue.symbol,
                          '[-1]',
                          sep = '')),
       sys.frame(sys.parent()))
  return(get('tmp', envir = temp.env))
}

queue <- function(vector, item)
{
  vector.lvalue.symbol <- substitute(vector)
  new.expression <- paste(vector.lvalue.symbol,
                          ' <- c(', vector.lvalue.symbol, ', ', item, ')',
                          sep = '')
  eval(parse(text = new.expression), sys.frame(sys.parent()))
}

dequeue <- function(vector)
{
  vector.lvalue.symbol <- substitute(vector)
  temp.env <- new.env()
  last.element <- eval(parse(text = paste(vector.lvalue.symbol,
                                          '[1]',
                                          sep = '')),
                       sys.frame(sys.parent()))
  assign('tmp', last.element, envir = temp.env)
  eval(parse(text = paste(vector.lvalue.symbol,
                          ' <- ', vector.lvalue.symbol,
                          '[-1]',
                          sep = '')),
       sys.frame(sys.parent()))
  return(get('tmp', envir = temp.env))
}
{% endhighlight %}

In general, the secret to writing these pseudo-macros is to use `substitute`. For the three functions that need to return a value as well as edit the passed parameter, you also need to use `new.env`, `assign` and `get` to edit the relevant symbol tables during function execution.

To check that these functions work, try the following examples after defining the functions above:

{% highlight r %}
v <- c(1)
push(v, 2)
v
pop(v) == 2
v
unshift(v, 0)
v
shift(v) == 0
v
queue(v, 2)
v
dequeue(v) == 1
v
{% endhighlight %}
