---
title: Some Pitfalls of Object Oriented Programming in R
author: John Myles White
layout: post
permalink: /notebook/2009/12/15/some-pitfalls-of-object-oriented-programming-in-r/
categories:
  - Observations
---

Yesterday, I wrote a post about implementing setter methods for objects in R. On my way to understanding how to implement these methods correctly, I made a bunch of mistakes. To keep people from wasting their time with these same mistakes, I'm reviewing them here.

### Mistake One: Naming Conventions

Every generic method will search for a class method using a single convention: if the generic method name is `GENERIC_METHOD` and the class name is `CLASS_NAME`, then the class method is always `GENERIC_METHOD.CLASS_NAME`. This is true no matter how strange the generic method looks: the generic method `id<-` for class `user` has a class method called `id<-.user`. The strange `<-` in the method name doesn't change the fact that `.user` comes at the end of the class method name.

### Mistake Two: Inspecting Existing Functions with Strange Names

If you want to get a sense how the function `class` works, you can simply type `class` at the R interpreter. Unfortunately, the parsing rules for R insure that this will never work for methods that end in "<-". For that reason, it's not at all clear how to get the definitions of these strangely named functions to come up in the interpreter. When defining these sort of methods, it's sufficient to simply enclose the method name, but that will not work for the purposes of introspection:

{% highlight r %}
'class<-'

# [1] "class<-"
{% endhighlight %}

As you can see, `'class<-'` evaluates to a string, which is neither surprising nor undesirable. Unfortunately, this leaves it unclear how to get at the definition of `class<-`. The secret for viewing the definitions of these strangely named functions is to enclose the function name in backticks:

{% highlight r %}
`class<-`

# function (x, value)  .Primitive("class<-")
{% endhighlight %}

### Mistake Three: Wasting Time on `<<-`

Before I learned that the trick to writing setter methods is to return an edited copy of the changed object, I was convinced that I needed to evaluate an assignment in the caller's scope. While pursuing this idea, I discovered the `<<-` operator. It's quite an interesting operator, but it's not the solution to the setter method definition problem.

You see, `<<-` is an assignment operator that will go searching through enclosing scopes (more precisely, R's environments) until it finds a variable that can be assigned to. Here's an example of how you could use `<<-` to do something surprising in R:

{% highlight r %}
i <- 1

"++" <- function()
{
  i <<- i + 1
}

"++"()

print(i)

# [1] 2

"++"()

print(i)

# [1] 3
{% endhighlight %}

This is pretty cool. In fact, it was cool enough that I wasted a good thirty minutes playing with it before realizing that it wasn't helping anything.

### Mistake Four: UseMethod is Magic

Figuring out how to write code that calls `UseMethod` for functions with many arguments is not trivial: `UseMethod`'s behavior is a little magical, and the R help docs are as spartan as ever with regard to calling `UseMethod`. If you naively try to write a function like this,

{% highlight r %}
i <- 2

class(i) <- 'myint'

raise.power <- function(x, power)
{
  UseMethod('raise.power', x, power)
}

raise.power.myint <- function(x, power)
{
  return(as.numeric(x ** power))
}

raise.power(i, 3)
{% endhighlight %}

you'll succeed at defining a generic method that generates warnings every single time it's run. This is because you are never allowed to pass more than two arguments to `UseMethod`. You see, `UseMethod` automatically discovers the arguments passed to its caller when it's called and otherwise completely ignores the arguments passed to it after the second argument. For that reason, what you need to write is

{% highlight r %}
i <- 2

class(i) <- 'myint'

raise.power <- function(x, power)
{
  UseMethod('raise.power', x)
}

raise.power.myint <- function(x, power)
{
  return(as.numeric(x ** power))
}

raise.power(i, 3)
{% endhighlight %}

with the latter arguments dropped from the call to `UseMethod`. If you're like me, you'll probably feel that you're going to lose `value` along the way the first few times that you write this. I suggest you try this approach out, convince yourself that it works, and then just take advantage of the `UseMethod` voodoo to solve your problems.
