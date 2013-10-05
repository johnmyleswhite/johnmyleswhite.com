---
title: Speeding Up MLE Code in R
author: John Myles White
layout: post
permalink: /notebook/2011/06/18/speeding-up-mle-code-in-r/
categories:
  - Economics
  - Programming
  - Psychology
  - Statistics
---

Recently, I've been fitting some models from the behavioral economics literature to choice data. Most of these models amount to non-linear variants of logistic regression in which I want to infer the parameters of a utility function. Because several of these models aren't widely used, I've had to write my own maximum likelihood code to estimate the parameters of these models.

In the process, I've started to learn something about how to write code that runs quickly in R. In this post, I'll try to share some of that knowledge by describing three ways of performing maximum likelihood estimation in R whose runtimes differ by two orders of magnitude. The differences seem to depend upon two factors: (1) how I access the entries of a data frame and (2) whether I use loops or vectorized operations to perform basic arithmetic.

To simplify things, I'll present a model that should be familiar to people with a background in economics: the exponentially discounted utility model. To implement it in R, we define the discounted value of `x` dollars at time `t` as:

{% highlight r %}
discounted.value <- function(x, t, delta)
{
  return(x * delta ^ t)
}
{% endhighlight %}

In addition to the discounted utility model, we assume that choices originate from a stochastic choice model with logistic noise. To invert this noise during inference, we'll use the inverse logit transform:

{% highlight r %}
invlogit <- function(z)
{
  return(1 / (1 + exp(-z)))
}
{% endhighlight %}

To test my inference routine, I need to generate "stochastic" data of the sort you would expect to see from an exponentially discounting agent that's indifferent between having $1 at time t = 0 and $3 at time t = 1. I'll refer to the first good as (X1, T1) and the second good as (X2, T2). If the agent chooses (X2, T2), I'll write that as `C == 1`; if they choose (X1, T1), I'll write that as `C == 0`. With those conventions, the sample data is generated as:

{% highlight r %}
n <- 100

choices <- data.frame(X1 = rep(1, each = n),
                      T1 = rep(0, each = n),
                      X2 = rep(3, each = n),
                      T2 = rep(1, each = n),
                      C = rep(c(0, 1), by = n / 2))
{% endhighlight %}

To fit the exponential model to this data set, we'll use the `optim` function to minimize the negative log likelihood of the data by setting two parameters: `a`, the variance of the noise in the utility function; and `delta`, the discount factor in the discounted utility model. The three implementations of this model that I'll show only differ in the definition of the log likelihood function, so the final call to `optim` to perform maximum likelihood estimation is constant across all examples:

{% highlight r %}
logit.estimator <- function(choices)
{ 
  wrapper <- function(x) {-log.likelihood(choices, x[1], x[2])}
  optimization.results <- optim(c(1, 1), wrapper, method = 'L-BFGS-B', lower = c(0, 0), upper = c(Inf, 1))
  return(optimization.results$par)
}
{% endhighlight %}

Here, I had to specify bounds for the parameters, `a` and `delta`, because it's assumed that `a` must be positive and that `delta` must lie in the interval [0, 1]. To deal with these bounds, one has to use the L-BFGS-B method in `optim`.

The first implementation I'll show is the one I find most natural to write, even though it turns out to be the least efficient by far:

{% highlight r %}
log.likelihood <- function(choices, a, delta)
{
  ll <- 0

  for (i in 1:nrow(choices))
  {
    u2 <- discounted.value(choices[i, 'X2'], choices[i, 'T2'], delta)
    u1 <- discounted.value(choices[i, 'X1'], choices[i, 'T1'], delta)

    p <- invlogit(a * (u2 - u1))

    if (choices[i, 'C'] == 1)
    {
      ll <- ll + log(p)
    }
    else
    {
      ll <- ll + log(1 - p)
    }
  }

  return(ll)
}
{% endhighlight %}

In the second implementation, I define a row level likelihood function, so that the summing and logarithmic transform are vectorized.

{% highlight r %}
rowwise.likelihood <- function(row, a, delta)
{
  u2 <- discounted.value(row['X2'], row['T2'], delta)
  u1 <- discounted.value(row['X1'], row['T1'], delta)
  p <- invlogit(a * (u2 - u1))
  return(ifelse(row['C'] == 1, p, 1 - p))
}

log.likelihood <- function(choices, a, delta)
{
  likelihoods <- apply(choices, 1, function (row) {rowwise.likelihood(row, a, delta)})
  return(sum(log(likelihoods)))
}
{% endhighlight %}

In the third implementation, I define a fully vectorized log likelihood function that avoids any explicit iteration and therefore removes most of the data frame indexing operations:

{% highlight r %}
log.likelihood <- function(choices, a, delta)
{
  u2 <- discounted.value(choices$X2, choices$T2, delta)
  u1 <- discounted.value(choices$X1, choices$T1, delta)
  p <- invlogit(a * (u2 - u1))
  likelihoods <- ifelse(choices$C == 1, p, 1 - p)
  return(sum(log(likelihoods)))
}
{% endhighlight %}

The code I used to call all of these implementations and compare them is up on [GitHub](https://github.com/johnmyleswhite/fastR) for those interested. The results, which strike me as remarkable, are below:

* On my laptop, implementation 1 takes ~1.0 second to run.
* On my laptop, implementation 2 takes ~0.25 seconds to run.
* On my laptop, implementation 3 takes ~0.01 seconds to run.

In short, the third implementation is 100x faster than the first implementation with only minor changes to the code I originally wrote. Hopefully this example will help inspire others who have R code they'd like to speed up, but aren't sure where to start.
