---
title: Times Series Methods versus Recurrence Relations
author: John Myles White
layout: post
permalink: /notebook/2009/12/10/times-series-methods-versus-recurrence-relations/
categories:
  - Statistics
---

This term, I've been sitting in on Rene Carmona's course on Modern Regression and Time Series Analysis. Much of the material on regression covered in the course was familiar to me already, but I've never felt that I had a real command of times series analysis methods.

When Carmona defined the [AR(p) model](http://en.wikipedia.org/wiki/Autoregressive_model) in class a few weeks ago, it struck me that, though I'd seen the defining equation several times before, I'd never realized earlier that the AR(p) model subsumes all possible linear recurrence relations. Also, the AR(p) model has the nice property that, if you already know the correct value of p, fitting the AR(p) model can be done with an ordinary least squares regression.

With these observations in mind, I decided to see how well I could derive the formulas for simple recurrence relations from a small data set. The results I got on my 2.4 GHz Intel Core 2 Duo MacBook are a useful case study in the dangers of naively using the default methods for fitting AR(p) models, as well as a particularly clear example of the inevitable inaccuracies in floating point arithmetic.

I hope [John D. Cook](http://www.johndcook.com/blog/2009/12/07/word-frequencies/) will forgive me for using the Fibonacci sequence as my example. While I totally agree with John that the Fibonacci sequence is not the ideal object to study if you're interested in day-to-day programming tasks, its simplicity makes it perfect for understanding how recurrence relations work.

The workhouse for fitting an AR(p) model in R is, predictably, the `ar` function. To see how well it would work for my purposes, I stored the first 15 Fibonacci terms in a vector and ran `ar` using all of its defaults settings. Here's the results:

{% highlight r %}
fibs <- c(1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610)

ar(fibs)

#Call:
#ar(x = fibs)
#
#Coefficients:
#     1  
#0.5922  
#
#Order selected 1  sigma^2 estimated as  21590 
{% endhighlight %}

These results are pretty terrible: the order for the model is chosen to be 1, which is clearly wrong. Given the wrong order, it's no surprise that the estimated coefficient is off, though it's strange that the result is so far off from the ideal coefficient for an order 1 model, which is `(1 + sqrt(5)) / 2`, or 1.618034. Thankfully, you can force `ar` to use the order you want by overriding some of the defaults:

{% highlight r %}
ar(fibs, order.max = 2, aic = FALSE)

#Call:
#ar(x = fibs, aic = FALSE, order.max = 2)
#
#Coefficients:
#      1        2  
# 0.6108  -0.0315  
#
#Order selected 2  sigma^2 estimated as  23366
{% endhighlight %}

You choose your preferred order using `order.max`, but this will only be an upper bound if you allow the function to use AIC scores to determine the order of the AR(p) model.

To figure out what was going wrong, I decided to use `lm` instead of `ar`. To do that, I needed subsets of my input data:

{% highlight r %}
fibs.1 <- fibs[1:(length(fibs) - 2)]
fibs.2 <- fibs[2:(length(fibs) - 1)]
fibs.3 <- fibs[3:length(fibs)]
{% endhighlight %}

Given these subset inputs, the call to `lm` is simple:

{% highlight r %}
lm(fibs.3 ~ fibs.1 + fibs.2)

#Call:
#lm(formula = fibs.3 ~ fibs.1 + fibs.2)
#
#Coefficients:
#(Intercept)       fibs.1       fibs.2  
#  2.365e-14    1.000e+00    1.000e+00  
{% endhighlight %}

As you can see, `lm` gets the right results, more or less. The non-zero intercept value is unfortunate, but suggests how easily floating point errors slip into these calculations.

Having gotten good results with `lm`, I decided to review `ar` a bit more: this led me to the conclusion that I should try using the `method = 'ols'` setting instead of `method = 'yule-walker'`.

{% highlight r %}
ar(fibs, order.max = 2, method = 'ols')

#Call:
#ar(x = fibs, order.max = 2, method = "ols")
#
#Coefficients:
#1  2  
#1  1  
#
#Intercept: 106.4 (6.715e-07) 
#
#Order selected 2  sigma^2 estimated as  6.276e-17 
{% endhighlight %}

This clearly works, though I find the output line about the intercept term confusing. I have to say that I'm a little surprised that the Yule-Walker method gives such bad results in this example: I'm not sure yet whether this is caused by the small sample size, a data set that can be fit without any error, intrinsic problems with the method, or something else I can't even conceive of.

Knowing that `ar` could work if the OLS method was enforced, I decided to try letting the AIC have its way again after reintroducing this method level constraint:

{% highlight r %}
ar(fibs, method = 'ols')

#Call:
#ar(x = fibs, method = "ols")
#
#Coefficients:
#1  2  
#1  1  
#
#Intercept: 106.4 (6.715e-07) 
#
#Order selected 2  sigma^2 estimated as  6.276e-17 
#Warning message:
#In ar.ols(x, aic = aic, order.max = order.max, na.action = na.action,  :
#  model order: 3singularities in the computation of the projection
#  matrixresults are only valid up to model order2
{% endhighlight %}

As you can see, this also works, though there's an error that I assume is a result of having input data that can be perfectly fit. In short, I think the take away lesson is that you can easily find the formula for recurrence relations using `ar` as long as you make sure you use ordinary least squares for fitting the various possible models.

Just to confirm that the OLS method would also find the ideal coefficient if forced to use an order 1 model, I ran `ar` one last time:

{% highlight r %}
ar(fibs, order.max = 1, method = 'ols')

#Call:
#ar(x = fibs, order.max = 1, method = "ols")
#
#Coefficients:
#     1  
#1.6182  
#
#Intercept: 65.74 (0.05852) 
#
#Order selected 1  sigma^2 estimated as  0.04309 
{% endhighlight %}

That result is satisfying and further confirms that the problems I had at the start are entirely attributable to using the Yule-Walker method with this data set.

**EDIT 12.11.2009: Replaced unfortunate term 'sparse' with non-overloaded word 'small'.**
