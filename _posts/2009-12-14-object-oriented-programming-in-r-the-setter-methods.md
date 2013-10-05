---
title: "Object-Oriented Programming in R: The Setter Methods"
author: John Myles White
layout: post
permalink: /notebook/2009/12/14/object-oriented-programming-in-r-the-setter-methods/
categories:
  - Statistics
---

With a little guidance from the indefatigable [Hadley Wickham](http://had.co.nz/), I figured out today how to implement the setter methods that were missing from my example user class. To review, let's rebuild the getter methods for my user object:

{% highlight r %}
user <- list(id = 1,
             password = '286755fad04869ca523320acce0dc6a4',
             email = 'jmw@johnmyleswhite.com')

class(user) <- 'user'

id <- function(x)
{
  UseMethod('id', x)
}

id.user <- function(user)
{
  return(user[['id']])
}

password <- function(x)
{
  UseMethod('password', x)
}

password.user <- function(user)
{
  return(user[['password']])
}

email <- function(x)
{
  UseMethod('email', x)
}

email.user <- function(user)
{
  return(user[['email']])
}
{% endhighlight %}

With these working, my goal was to write setter methods that would work like the ideal code below:

{% highlight r %}
id(user) <- 2

if (id(user) == 2)
{
  print("Succeeded in editing the user's id attribute.")
}
{% endhighlight %}

Defining this sort of setter method turned out to require a little effort. I wasted quite a lot of time walking down dead ends, but, thankfully, Hadley gave me the exact piece of information I was missing early this morning. The dead ends were interesting in themselves, though, so I'll review them in another post tomorrow.

For now I'll just explain the correct implementation for my desired setter methods, which you can see below:

{% highlight r %}
'id<-' <- function(x, value)
{
  UseMethod('id<-', x)
}

'id<-.user' <- function(x, value)
{
  x[['id']] <- value
  return(x)
}
{% endhighlight %}

There are three important ideas here:

* The generic setter method for `id` should be called `'id<-'`, which must always be quoted to prevent parsing errors.
* The class level setter method should be called `'id<-.user'`, which isn't surprising, though I had imagined it might be called `'id.user<-'` at one point.
* The setter method should make a change to a copy of the original object and return the edited copy to the caller. R handles the assignment of this edited return value to the original object behind the scenes. In fact, not using this return value approach will make otherwise plausible looking code fail to edit your object correctly. I stumbled on that for quite a while before I was shown the light.

With this in mind, a final user class looks like this:

{% highlight r %}
user <- list(id = 1,
             password = '286755fad04869ca523320acce0dc6a4',
             email = 'jmw@johnmyleswhite.com')

class(user) <- 'user'

id <- function(x)
{
  UseMethod('id', x)
}

id.user <- function(user)
{
  return(user[['id']])
}

password <- function(x)
{
  UseMethod('password', x)
}

password.user <- function(user)
{
  return(user[['password']])
}

email <- function(x)
{
  UseMethod('email', x)
}

email.user <- function(user)
{
  return(user[['email']])
}

'id<-' <- function(x, value)
{
  UseMethod('id<-', x)
}

'id<-.user' <- function(x, value)
{
  x[['id']] <- value
  return(x)
}

'password<-' <- function(x, value)
{
  UseMethod('password<-', x)
}

'password<-.user' <- function(x, value)
{
  x[['password']] <- value
  return(x)
}

'email<-' <- function(x, value)
{
  UseMethod('email<-', x)
}

'email<-.user' <- function(x, value)
{
  x[['email']] <- value
  return(x)
}
{% endhighlight %}
