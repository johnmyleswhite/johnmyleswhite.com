---
title: Useful One-Line for Mac OS X Users
author: John Myles White
layout: post
permalink: /notebook/2011/01/11/useful-one-line-for-mac-os-x-users/
categories:
  - Programming
---

Today I needed to quickly reverse the order of rows in a LaTeX table. This command nicely did the trick:

{% highlight bash %}
pbpaste | awk '{ line[NR] = $0 } END { for (i=NR;i>0;i--) print line[i] }'
{% endhighlight %}

I stole the awk piece [from here](http://groups.engin.umd.umich.edu/CIS/course.des/cis400/awk/reverse.txt).
