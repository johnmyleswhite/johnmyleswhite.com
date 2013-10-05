---
title: Problems with ggplot2 0.8.9 and R 2.13.0 on Mac OS X via plyr 1.5
author: John Myles White
layout: post
permalink: /notebook/2011/04/14/problems-with-ggplot2-0-8-9-and-r-2-13-0-on-mac-os-x-via-plyr-1-5/
categories:
  - Mac OS X
  - Programming
  - Statistics
---

This morning I tried to completely update my R installation. I first dumped a list of all the packages I have on my system using the `installed.packages()` function. Then I installed R 2.13.0 using the OS X disk image. And finally I reinstalled all of my packages from scratch.

Unfortunately, I ran into some serious problems along the way. After installing everything from scratch, 'ggplot2' 0.8.9 was broken. Specifically, I couldn't get error bars to work with `stat_summary()`. For example, this code wouldn't work on my system:

{% highlight r %}
# Problem with ggplot2 Version "0.8.9"

library('ggplot2')

set.seed(1)

example.data <- data.frame(Measurement = rnorm(5, 0, 1), Class = rep('A', 5))

ggplot(example.data, aes(x = Class, y = Measurement)) +
  stat_summary(fun.data = 'mean_cl_boot', geom = 'bar') +
  stat_summary(fun.data = 'mean_cl_boot', geom = 'errorbar')
{% endhighlight %}

Thankfully, I managed to enlist [Dirk Eddelbuettel's](http://dirk.eddelbuettel.com/blog/) help through Twitter and he ran the code on his own recently updated system. Things worked fine for him, which suggested that the problem was in my system configuration. We compared package versions and discovered that he had 'plyr' 1.5.1 on his Ubuntu machine, while I had 'plyr' 1.5 on my OS X machine. After looking at CRAN, it was clear that the Mac OS X build wasn't available on CRAN yet.

To fix this, I grabbed the source for 'plyr' 1.5.1 and tried to install it myself. That led to the following error:

{% highlight bash %}
** preparing package for lazy loading
Error: package 'plyr' is required by 'reshape' so will not be detached
{% endhighlight %}

The problem was that 'reshape' was being loaded automatically when R was starting up. Since 'reshape' depends on 'plyr', R wasn't willing to overwrite my old 'plyr' 1.5 with the new 'plyr' 1.5.1. The solution was to edit my `.Rprofile` file to prevent 'reshape' from being autoloaded. Once I did this, I was able to run the standard `R CMD INSTALL` and get the new version of 'plyr' on my system. And after that 'ggplot2' 0.8.9 started working properly.

Hopefully no one else will come up against the same issue after the binary for 'plyr' 1.5.1 gets pushed through all of the CRAN mirrors. But if you get errors while using 'ggplot2' 0.8.9, look into installing 'plyr' 1.5.1 from source on your system.

Many thanks to Dirk for giving me so much help today.
