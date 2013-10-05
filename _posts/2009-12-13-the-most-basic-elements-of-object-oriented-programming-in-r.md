---
title: The Most Basic Elements of Object-Oriented Programming in R
author: John Myles White
layout: post
permalink: /notebook/2009/12/13/the-most-basic-elements-of-object-oriented-programming-in-r/
categories:
  - Statistics
---

Until recently, I've never had any reason to learn how to define my own classes in R. Having learned this week, I was surprised to find out how easy it is to start implementing classes in R. If you know nothing about creating classes and class methods in R, here's a very quick overview of the three core ideas behind R's object system. If you already know the basics of defining classes in R, I'd suggest skipping this post, as it's not likely to be of much value to you.

The first thing you should know is that the S3 object-oriented system feels like a hack that was put on top of the original S language. If you're familiar with Perl, you'll feel right at home with the basic ideas behind this approach; you'll probably also appreciate the conceptual simplicity of the results.

With that in mind, you should be aware that any normal piece of R code is filled with objects, because the base packages and most well-designed user packages use lots of custom classes. You can see this for yourself by starting to use `class` to perform introspection on some of the items in your programs:

{% highlight r %}
class(1)
# [1] "numeric"

class('a')
# [1] "character"

x <- 1:10
y <- 5 * x + rnorm(length(x), 0, 1)

fit <- lm(y ~ x)

class(fit)
# [1] "lm"
{% endhighlight %}

More interesting than looking at the classes of existing objects is defining new classes of your own. Thankfully, `class`, like `names` and many other functions in R, can be assigned to directly, like so:

{% highlight r %}
a <- 1

class(a) <- 'octonion'

class(a)
# [1] "octonion"
{% endhighlight %}

If you're familiar with Perl, you can think of this assignment to `class` as analogous to using `bless`. If you're not a Perl user, hopefully this approach to defining classes still makes sense to you, because it's so simple: the only thing that decides the class of an object is the value you've set for the class attribute. To convince yourself that the class of an object is just an attribute, you can use `attr`:

{% highlight r %}
attr(a, 'class')
{% endhighlight %}

This simple metadata driven approach to building objects is the core of the S3 object system, as far as I can tell.

Of course, setting `class` by itself is pretty useless: you want to be able to define class methods. That's where the other two main ideas come in. The first trick is to use generic methods to get polymorphism out of the language; the second trick is to define methods on your classes using a simple naming convention. To see how this works, let's use an example that should be familiar to anyone who's ever built a database-backed website.

Instead of my would-be octonion class, we'll consider a user object. To define an object of class 'user', we can do the following:

{% highlight r %}
user <- list(id = 1,
             password = '41bfe136a536b7749104415bc364df2e',
             email = 'jmw@johnmyleswhite.com')

class(user) <- 'user'

user

#$id
#[1] 1
#
#$password
#[1] "41bfe136a536b7749104415bc364df2e"
#
#$email
#[1] "jmw@johnmyleswhite.com"
#
#attr(,"class")
#[1] "user"
{% endhighlight %}

The first thing that comes to mind after building this object is that we should define getter and setter methods for accessing and modifying the contents of this user object. For example, we want a way to get the id attribute for our object. We'd also like our approach to generalize to other objects with an id attribute. Ideally, we should be able to write something like this:

{% highlight r %}
id(user)
{% endhighlight %}

Now, it is obviously possible to define an `id` function that operates only on user objects, but that's not the right approach if we also want to have another sort of object that would have an `id` method as well. This polymorphism concern is the problem that generic methods and specialized naming conventions solve.

First, we are going to define a specialized `id` method that only operates on user objects. Because it will only work on 'user' objects, we'll call it `id.user`:

{% highlight r %}
id.user <- function(user.object)
{
	return(user.object[['id']])
}

id.user(user)
# [1] 1
{% endhighlight %}

Given this, we can then define a generic method that will operate on objects of many classes and reroute our general function calls to the correct class-level method:

{% highlight r %}
id <- function(object)
{
	UseMethod('id', object)
}

id(user)
# [1] 1
{% endhighlight %}

Here `UseMethod` just searches for an `id.user` function, finds it and then calls it with `user` as its argument. If we had a 'profile' object, `id` would search for an `id.profile` function and call that. You can see this by trying `id` on a variable whose class we set to be `profile` using `class`.

{% highlight r %}
profile <- list(id = 1)

class(profile) <- 'profile'

id(profile)

# Error in UseMethod("id", user) : 
#  no applicable method for 'id' applied to an object of class "profile"
{% endhighlight %}

With these ideas in mind, it's easy to do something similar for the rest of our attributes as well:

{% highlight r %}
password.user <- function(user.object)
{
	return(user.object[['password']])
}

password <- function(user)
{
	UseMethod('password', user)
}

email.user <- function(user.object)
{
	return(user.object[['email']])
}

email <- function(user)
{
	UseMethod('email', user)
}
{% endhighlight %}

Defining setter methods is a little more tricky. We could use a trick like the `eval` hacks I used to implement `push` and `pop` recently, but Hadley Wickham wisely chastened me for doing that. I'm still trying to decide what's the best approach.

As I see it, there are least three possible method call styles you might define:

{% highlight r %}
user <- password(user, 'new_value')

password(user, 'new_value')

password(user) <- 'new_value'
{% endhighlight %}

I would really like to start implementing this last sort, but I haven't figured out how to yet. The others can be implemented using the generic methods approach I just outlined above, along with some ugly calls to `eval` for the second call style. To implement the first, simply do something like this:

{% highlight r %}
password.user <- function(user, new.value = NULL)
{
  if (is.null(new.value))
  {
    return(user[['password']])
  }
  else
  {
    user[['password']] <- new.value
    return(user)
  }
}

password <- function(user, new_value = NULL)
{
  UseMethod('password', user, new_value)
}
{% endhighlight %}

If you have suggestions on how to implement the third approach, please let me know. Also, I am not at all happy with the use of `NULL` in my current setter implementation, because that makes it impossible to set the value of an attribute to `NULL`. If people have suggestions, I'd very much appreciate them. Does R implement arity-specific function definitions?
