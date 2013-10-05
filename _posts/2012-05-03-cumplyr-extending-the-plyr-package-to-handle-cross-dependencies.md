---
title: "cumplyr: Extending the plyr Package to Handle Cross-Dependencies"
author: John Myles White
layout: post
permalink: /notebook/2012/05/03/cumplyr-extending-the-plyr-package-to-handle-cross-dependencies/
categories:
  - Programming
  - Statistics
---

### Introduction

For me, [Hadley Wickham](http://had.co.nz/)'s [reshape](http://had.co.nz/reshape/) and [plyr](http://plyr.had.co.nz/) packages are invaluable because they encapsulate omnipresent design patterns in statistical computing: reshape handles switching between the different possible representations of the same underlying data, while plyr automates what Hadley calls the [Split-Apply-Combine strategy](http://www.jstatsoft.org/v40/i01/paper), in which you split up your data into several subsets, perform some computation on each of these subsets and then combine the results into a new data set. Many of the computations implicit in traditional statistical theory are easily described in this fashion: for example, comparing the means of two groups is computationally equivalent to splitting a data set of individual observations up into subsets based on the group assignments, applying mean to those subsets and then pooling the results back together again.

### The Split-Apply-Combine Strategy is Broader than plyr

The only weakness of plyr, which automates so many of the computations that instantiate the Split-Apply-Combine strategy, is that plyr implements one very specific version of the Split-Apply-Combine strategy: plyr always splits your data into disjoint subsets. By disjoint, I mean that any row of the original data set can occur in only one of the subsets created by the splitting function. For computations that involve cross-dependencies between observations, this makes plyr inapplicable: cumulative quantities like running means and broadly local quantities like kernelized means cannot be computed using plyr. To highlight that concern, let's consider three very simple data analysis problems.

#### Computing Forward-Running Means

Suppose that you have the following data set:

<table class="table table-hover">
  <tr><th>Time</th><th>Value</th></tr>
  <tr><td>1</td><td>1</td></tr>
  <tr><td>2</td><td>3</td></tr>
  <tr><td>3</td><td>5</td></tr>
</table>

To compute a forward-running mean, you need to split this data into three subsets:

<table class="table table-hover">
  <tr><th>Time</th><th>Value</th></tr>
  <tr><td>1</td><td>1</td></tr>
</table>

<table class="table table-hover">
  <tr><th>Time</th><th>Value</th></tr>
  <tr><td>1</td><td>1</td></tr>
  <tr><td>2</td><td>3</td></tr>
</table>

<table class="table table-hover">
  <tr><th>Time</th><th>Value</th></tr>
  <tr><td>1</td><td>1</td></tr>
  <tr><td>2</td><td>3</td></tr>
  <tr><td>3</td><td>5</td></tr>
</table>

In each of these clearly non-disjoint subsets, you would then compute the mean of `Value` and combine the results to give:

<table class="table table-hover">
  <tr><th>Time</th><th>Value</th></tr>
  <tr><td>1</td><td>1</td></tr>
  <tr><td>2</td><td>2</td></tr>
  <tr><td>3</td><td>3</td></tr>
</table>

This sort of computation occurs often enough in a simpler form that R provides tools like `cumsum` and `cumprod` to deal with cumulative quantities. But the splitting problem in our example is not addressed by those tools, nor by plyr, because the cumulative quantities have to computed on subsets that are not disjoint.

#### Computing Backward-Running Means

Consider performing the same sort of calculation as described above, but moving in the opposite direction. In that case, the three non-disjoint subsets are:

<table class="table table-hover">
  <tr><th>Time</th><th>Value</th></tr>
  <tr><td>3</td><td>5</td></tr>
</table>

<table class="table table-hover">
  <tr><th>Time</th><th>Value</th></tr>
  <tr><td>2</td><td>3</td></tr>
  <tr><td>3</td><td>5</td></tr>
</table>

<table class="table table-hover">
  <tr><th>Time</th><th>Value</th></tr>
  <tr><td>1</td><td>1</td></tr>
  <tr><td>2</td><td>3</td></tr>
  <tr><td>3</td><td>5</td></tr>
</table>

And the final result is:

<table class="table table-hover">
  <tr><th>Time</th><th>Value</th></tr>
  <tr><td>1</td><td>3</td></tr>
  <tr><td>2</td><td>4</td></tr>
  <tr><td>3</td><td>5</td></tr>
</table>

#### Computing Local Means (AKA Kernelized Means)

Imagine that, instead of looking forward or backward, we only want to know something about data that is close to the current observation being examined. For example, we might want to know the mean value of each row when pooled with its immediately proceeding and succeeding neighbors. This computation must create the following subsets of data:

<table class="table table-hover">
  <tr><th>Time</th><th>Value</th></tr>
  <tr><td>1</td><td>1</td></tr>
  <tr><td>2</td><td>3</td></tr>
</table>

<table class="table table-hover">
  <tr><th>Time</th><th>Value</th></tr>
  <tr><td>1</td><td>1</td></tr>
  <tr><td>2</td><td>3</td></tr>
  <tr><td>3</td><td>5</td></tr>
</table>

<table class="table table-hover">
  <tr><th>Time</th><th>Value</th></tr>
  <tr><td>2</td><td>3</td></tr>
  <tr><td>3</td><td>5</td></tr>
</table>

Within these non-disjoint subsets, means are computed and the result is:

<table class="table table-hover">
  <tr><th>Time</th><th>Value</th></tr>
  <tr><td>1</td><td>2</td></tr>
  <tr><td>2</td><td>3</td></tr>
  <tr><td>3</td><td>4</td></tr>
</table>

### A Strategy for Handling Non-Disjoint Subsets

How can we build a general purpose tool to handle these sorts of computations? One way is to rethink how plyr works and then extend it with some trivial variations on its core principles. We can envision plyr as a system that uses a splitting operation that partitions our data into subsets in which each subset satisfies a group of equality constraints: you split the data into groups in which `Variable 1 = Value 1 AND Variable 2 = Value 2`, etc. Because you consider the conjunction of several equality constraints, the resulting subsets are disjoint.

Seen in this fashion, there is a simple relaxation of the equality constraints that allows us to solve the three problems described a moment ago: instead of looking at the conjunction of equality constraints, we use a conjunction of inequality constraints. For the time being, I'll describe just three instantiations of this broader strategy.

### Using Upper Bounds

Here, we divide data into groups in which `Variable 1 <= Value 1 AND Variable 2 <= Value 2`, etc. We will also allow equality constraints, so that the operations of plyr are a strict subset of the computations in this new model. For example, we might use the constraint `Variable = Value 1 AND Variable 2 <= Value 2`. If the upper bound is the `Time` variable, these contraints will allow us to compute the forward-moving mean we described earlier.

### Using Lower Bounds

Instead of using upper bounds, we can use lower bounds to divide data into groups in which `Variable >= Value 1 AND Variable 2 >= Value 2`, etc. This allows us to implement the backward-moving mean described earlier.

### Using Norm Balls

Finally, we can consider a combination of upper and lower bounds. For simplicity, we'll assume that these bounds have a fixed tightness around the "center" of each subset of our split data. To articulate this tightness formally, we look at a specific hypothetical equality constraint like `Variable 1 = Value 1` and then loosen it so that `norm(Variable 1 - Value 1) <= r`. When `r = 0`, this system gives the original equality constraint. But when `r > 0`, we produce a "ball" of data around the constraint whose tightness is `r`. This lets us estimate the local means from our third example.

### Implementation

To demo these ideas in a usable fashion, I've created a draft package for R called [cumplyr](http://bit.ly/IEPGnW). Here is an extended example of its usage in solving simple variants of the problems described in this post:

{% highlight r %}
library('cumplyr')

data <- data.frame(Time = 1:5, Value = seq(1, 9, by = 2))

iddply(data,
       equality.variables = c('Time'),
       lower.bound.variables = c(),
       upper.bound.variables = c(),
       norm.ball.variables = list(),
       func = function (df) {with(df, mean(Value))})

iddply(data,
       equality.variables = c(),
       lower.bound.variables = c('Time'),
       upper.bound.variables = c(),
       norm.ball.variables = list(),
       func = function (df) {with(df, mean(Value))})

iddply(data,
       equality.variables = c(),
       lower.bound.variables = c(),
       upper.bound.variables = c('Time'),
       norm.ball.variables = list(),
       func = function (df) {with(df, mean(Value))})

iddply(data,
       equality.variables = c(),
       lower.bound.variables = c(),
       upper.bound.variables = c(),
       norm.ball.variables = list('Time' = 1),
       func = function (df) {with(df, mean(Value))})

iddply(data,
       equality.variables = c(),
       lower.bound.variables = c(),
       upper.bound.variables = c(),
       norm.ball.variables = list('Time' = 2),
       func = function (df) {with(df, mean(Value))})

iddply(data,
       equality.variables = c(),
       lower.bound.variables = c(),
       upper.bound.variables = c(),
       norm.ball.variables = list('Time' = 5),
       func = function (df) {with(df, mean(Value))})
{% endhighlight %}

You can download this package from [GitHub](http://bit.ly/IEPGnW) and play with it to see whether it helps you. Please submit feedback using GitHub if you have any comments, complaints or patches.

### Comparing plyr with cumplyr

In the long run, I'm hoping to make the functions in [cumplyr](http://bit.ly/IEPGnW) robust enough to submit a patch to plyr. I see these tools as one logical extension of plyr to encompass more of the framework described in Hadley's paper on the Split-Apply-Combine strategy.

For the time being, I would advise any users of [cumplyr](http://bit.ly/IEPGnW) to make sure that you do not use cumplyr for anything that plyr could already do. cumplyr is very much demo software and I am certain that both its API and implementation will change. In contrast, plyr is fast and stable software that can be trusted to perform its job.

But, if you have a problem that cumplyr will solve and plyr will not, I hope you'll try cumplyr out and submit patches when it breaks.

Happy hacking!
