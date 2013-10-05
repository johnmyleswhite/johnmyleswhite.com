---
title: Floating Point Arithmetic and The Descent into Madness
author: John Myles White
layout: post
permalink: /notebook/2012/04/13/floating-point-arithmetic-and-the-descent-into-madness/
categories:
  - Programming
  - Statistics
---

While I should confess upfront that I've always had a weaker command of the details of floating point arithmetic than I feel I ought to have, this sort of thing still blows my mind when I stumble upon it. These moments invariably make me realize that floating point math will simply never satisfy my naive hopes as a mathematician:

{% highlight julia %}
0.1 + 0.1 == 0.2 # True
0.1 + 0.1 + 0.1 == 0.3 # False
0.1 + 0.1 + 0.1 + 0.1 == 0.4 # True
{% endhighlight %}

On my Intel Core 2 Duo machine running OS X, those statements have the indicated truth values in all three of Julia, R and Python.

Consider this evidence for the truth of the combined propositions, "God created the integers. All else is the work of man," and "Out of the crooked timber of humanity no straight thing was ever made."
