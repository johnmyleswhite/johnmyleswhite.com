---
title: "Why You Shouldn't Be Clever"
author: John Myles White
layout: post
permalink: /notebook/2008/08/20/why-you-shouldnt-be-clever/
categories:
  - Ruby
---

Today I started reading the [Ruby Snips](http://rubysnips.com
) website, which has a pretty good sample of interesting snippets of Ruby code on it. I was particularly intrigued by the following snippet from a post on [Prime Numbers](http://rubysnips.com/prime-mixin) dating back to March 23rd, 2007:

{% highlight ruby %}
class Fixnum
  def prime?
    ('1' * self) !~ /^1?$|^(11+?)\1+$/
  end
end
{% endhighlight %}

At first, I was convinced this code was broken. Before I had given much thought to the algorithm, I was ready to assume that the "prime?" name was a misnomer: I assumed the code was actually testing for members of a specific class of palindromes. After a minute or two, I pieced together what was really happening when testing a number:

* The number, n, is expanded into a string of 1's of length n.
* The string of 1's is testing using a regex with back-references that finds the presence of a repeated divisor, a technique that works because a string of length a * b = n is composed of b copies of a string of length a.

At that point, I began to marvel at the simplicity of the algorithm relative to the obscurity of the code. And while I was so amazed by this, it occurred to me: this is an absolutely terrible way to test for primality. Because you have no control over the loop bounds for the regex, you effectively test every number up to n as a possible divisor if n is a prime. But you really only need to test up to the square root of n to determine if n is prime. So your code runs for the square of the time it should run. And the expansion of n into a string, at least theoretically, requires exponentially more space than an integer expressed in binary would. This seemed like one of the worst possible ways to test for primality I had ever seen.

Still, I wanted to verify this empirically as well as theoretically, so I typed in

{% highlight ruby %}
399839483.prime?
{% endhighlight %}

during an IRB session. The CPU and memory usage were absurd for a primality test on such a small number. I didn't even bother letting the code complete after a few seconds of the watching interpreter hanging there, silently wasting CPU cycles. In contrast, the simpler, clearer code,

{% highlight ruby %}
class Fixnum
  def prime?
    (2..Math.sqrt(self).to_i).each do |i|
      if i * (self / i) == self
        return false
      end
    end
    return true
  end
end
{% endhighlight %}

runs fairly quickly.

The lesson I think everyone should take from this is a simple one: don't be clever when writing code. At the very least, don't be clever in the way the author of this snippet was. Your code is likely to end up being unclear, slow and wasteful with memory.

Perhaps I should be clearer about the problems of cleverness. Clever code is a mistake; clever algorithms are wonderful. Quick sort is a clever algorithm that always confuses me until I think carefully about it -- and then I marvel at its superiority over other less efficient sorting algorithms; this snippet is a mediocre algorithm written in a style of code that's needlessly confusing.
