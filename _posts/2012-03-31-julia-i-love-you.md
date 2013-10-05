---
title: Julia, I Love You
author: John Myles White
layout: post
permalink: /notebook/2012/03/31/julia-i-love-you/
categories:
  - Statistics
---

[Julia](http://julialang.org/) is a new language for scientific computing that is winning praise from a slew of very smart people, including [Harlan Harris](https://en.twitter.com/#!/HarlanH/status/173478216781144065), [Chris Fonnesbeck](https://de.twitter.com/#!/fonnesbeck/status/185399199976787968), [Douglas Bates](http://dmbates.blogspot.com/2012/03/julia-version-of-multinomial-sampler_12.html), [Vince Buffalo](http://vincebuffalo.org/2012/03/07/thoughts-on-julia.html) and [Shane Conway](http://www.statalgo.com/2012/03/24/statistics-with-julia/). As a language, it has lofty design goals, which, if attained, will make it noticeably superior to Matlab, R and Python for scientific programming. In the core development team's [own words](http://julialang.org/blog/2012/02/why-we-created-julia/):

<blockquote>
<p>We want a language that's open source, with a liberal license. We want the speed of C with the dynamism of Ruby. We want a language that's homoiconic, with true macros like Lisp, but with obvious, familiar mathematical notation like Matlab. We want something as usable for general programming as Python, as easy for statistics as R, as natural for string processing as Perl, as powerful for linear algebra as Matlab, as good at gluing programs together as the shell. Something that is dirt simple to learn, yet keeps the most serious hackers happy. We want it interactive and we want it compiled.</p>

<p>(Did we mention it should be as fast as C?)</p>
</blockquote>

Remarkably, Julia seems to be on its way to meeting those goals. Last night, I decided to see for myself whether Julia would live up to the hype. So I taught myself just enough of the language to write an implementation of the slowest R code I've ever written: the Metropolis algorithm-style sampler Drew and I use in Chapter 7 of [Machine Learning for Hackers](http://shop.oreilly.com/product/0636920018483.do) to show off randomized, iterative optimization algorithms. You can find both the original R code and my new Julia code on [GitHub](https://github.com/johnmyleswhite/JuliaVsR) in two files name `cipher.R` and `cipher.jl`, respectively.

In my opinion, the new code in Julia is easier to read than the R code because Julia has fewer syntactic quirks than R. More importantly, the Julia code runs much faster than the R code without any real effort put into speed optimization. For the sample text I tried to decipher, the Julia code completes 50,000 iterations of the sampler in 51 seconds, while the R code completes the same 50,000 iterations in 67 minutes -- making the R code more than 75 slower than the Julia code.

Having seen that example alone, I would be convinced Julia is a real contender for the future of scientific computing. But this iterative sampling algorithm is not close to being the harshest comparison between Julia and R on my machine. For a more powerful example (lifted straight from the Julia docs), we can compare Julia and R code for computing the 25th Fibonacci number recursively.

First, the Julia code:

{% highlight julia %}
fib(n) = n < 2 ? n : fib(n - 1) + fib(n - 2)
@elapsed fib(25)
{% endhighlight %}

Second, the R code:

{% highlight r %}fib <- function(n)
{
  ifelse(n < 2, n, fib(n - 1) + fib(n - 2))
}

start <- Sys.time()
fib(25)
end <- Sys.time()
end - start
{% endhighlight %}

The Julia code takes around 8 milliseconds to complete, whereas the R code takes around 4000 milliseconds. In this case, R is 500 times slower than Julia. To me, that's sufficient reason to want to start focusing my time on implementing the algorithms I care about in Julia. I hope others will consider doing the same.
