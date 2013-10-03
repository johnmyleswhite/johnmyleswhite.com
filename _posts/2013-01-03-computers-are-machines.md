---
title: Computers are Machines
author: John Myles White
layout: post
permalink: /notebook/2013/01/03/computers-are-machines/
categories:
  - Programming
  - Statistics
---

When people try out [Julia](http://julialang.org) for the first time, many of them are worried by the following example:

{% highlight julia %}
factorial(n) = n == 0 ? 1 : n * factorial(n - 1)
 
factorial(20) # => 2432902008176640000
 
factorial(21) # => -4249290049419214848
{% endhighlight %}

If you're not familiar with computer architecture, this result is very troubling. *Why would Julia claim that the factorial of 21 is a negative number?*

The answer is simple, but depends upon a set of concepts that are largely unfamiliar to programmers who, like me, grew up using modern languages like Python and Ruby. Julia thinks that the factorial of 21 is a negative number because *computers are machines*.

Because they are machines, computers represent numbers using many small groups of bits. Most modern machines work with groups of 64 bits at a time. If an operation has to work with more than 64 bits at a time, that operation will be slower than a similar operation than only works with 64 bits at a time.

As a result, if you want to write fast computer code, it helps to only execute operations that are easily expressible using groups of 64 bits.

Arithmetic involving small integers fits into the category of operations that only require 64 bits at a time. Every integer between `-9223372036854775808` and `9223372036854775807` can be expressed using just 64 bits. You can see this for yourself by using the `typemin` and `typemax` functions in Julia:

{% highlight julia %}
typemin(Int64) # => -9223372036854775808
 
typemax(Int64) # => 9223372036854775807
{% endhighlight %}

If you do things like the following, the computer will quickly produce correct results:

{% highlight julia %}
typemin(Int64) + 1 # => -9223372036854775807
 
typemax(Int64) - 1 # => 9223372036854775806
{% endhighlight %}

But things go badly if you try to break out of the range of numbers that can be represented using only 64 bits:

{% highlight julia %}
typemin(Int64) - 1 # => 9223372036854775807
 
typemax(Int64) + 1 # => -9223372036854775808
{% endhighlight %}

The reasons for this are not obvious at first, but make more sense if you examine the actual bits being operated upon:

{% highlight julia %}
bits(typemax(Int64))
# => "0111111111111111111111111111111111111111111111111111111111111111"
 
bits(typemax(Int64) + 1)
# => "1000000000000000000000000000000000000000000000000000000000000000"
 
bits(typemin(Int64))
# => "1000000000000000000000000000000000000000000000000000000000000000"
{% endhighlight %}

When it adds 1 to a number, the computer blindly uses a simple arithmetic rule for individual bits that works just like the carry system you learned as a child. This carrying rule is very efficient, but works poorly if you end up flipping the very first bit in a group of 64 bits. The reason is that this first bit represents the sign of an integer. When this special first bit gets flipped by an operation that overflows the space provided by 64 bits, everything else breaks down.

The special interpretation given to certain bits in a group of 64 is the reason that factorial of 21 is a negative number when Julia computes it. You can confirm this by looking at the exact bits involved:

{% highlight julia %}
bits(factorial(20))
# => "0010000111000011011001110111110010000010101101000000000000000000"
 
bits(factorial(21))
# => "1100010100000111011111010011011010111000110001000000000000000000"
{% endhighlight %}

Here, as before, the computer has just executed the operations necessary to perform multiplication by 21. But the result has flipped the sign bit, which causes the result to appear to be a negative number.

There is a way around this: you can tell Julia to work with groups of more than 64 bits at a time when expressing integers using the `BigInt` type:

{% highlight julia %}
require("BigInt")
 
BigInt(typemax(Int)) # => 9223372036854775807
 
BigInt(typemax(Int)) + 1 # => 9223372036854775808
 
BigInt(factorial(20)) * 21 # => 51090942171709440000
{% endhighlight %}

Now everything works smoothly. By working with `BigInt`&#8216;s automatically, languages like Python avoid these concerns:

{% highlight python %}
factorial(20) # => 2432902008176640000

factorial(21) # => 51090942171709440000L
{% endhighlight %}

The `L` at the end of the numbers here indicates that Python has automatically converted a normal integer into something like Julia's `BigInt`. But this automatic conversion comes at a substantial cost: every operation that stays within the bounds of 64-bit arithmetic is slower in Python than Julia because of the time required to check whether an operation might go beyond the 64-bit bound.

Python's automatic conversion approach is safer, but slower. Julia's approach is faster, but requires that the programmer understand more about the computer's architecture. Julia achieves its performance by confronting the fact that computers are machines head on. This is confusing at first and frustrating at times, but it's a price that you have to pay for high performance computing. Everyone who grew up with C is used to these issues, but they're largely unfamiliar to programmers who grew up with modern languages like Python. In many ways, Julia sets itself apart from other new languages by its attempt to recover some of the power that was lost in the transition from C to languages like Python. But the transition comes with a substantial learning curve.

And that's why I wrote this post.
