---
title: Implementing the Exact Binomial Test in Julia
author: John Myles White
layout: post
permalink: /notebook/2012/04/14/implementing-the-exact-binomial-test-in-julia/
categories:
  - Programming
  - Statistics
---

One major benefit of spending my time recently adding [statistical functionality to Julia](https://github.com/johnmyleswhite/stats.jl) is that I've learned a lot about the inner guts of algorithmic null hypothesis significance testing.

Implementing [Welch's two-sample t-test](https://github.com/johnmyleswhite/stats.jl/blob/master/src/t_test.jl) last week was a trivial task because of the symmetry of the null hypothesis, but implementing the exact binomial test has proven to be more challenging because the asymmetry of a skewed null defined over a bounded set means that one has to think a bit more carefully about what is being computed.

To see why, let's first recap the logic of the standard two-sided hypothesis test. In all NHST situations, you assign a p-value to the observed data by working under the null hypothesis and using this assumption to calculate the probability of observing data sets that are as extreme or more extreme than the observed data.

For the normal, this calculation is easy: you find the probability of seeing an equal or higher z-score than the observed data's z-score and then you double this probability to account for the lower tail in which the hypothetical z-score is lower than the observed z-score.

But for the binomial, the right quantity to use as a definition of extremity is less obvious (at least to me). Suppose that you've seen `x` successes after `n` samples from a Bernoulli variable with probability `p` of success.

You might try defining extremity by saying that a hypothetical data set `y` is more extreme than `x` if `abs(y - n) > abs(x - n)`: in short, you could use the count space to assess extremity.

This approach will not work. Consider the case in which `x = 4`, `n = 10` and `p = 0.2`. Under this definition `y = 0` would be as extreme as `y = 4`, but `p(y = 0) > p(y = 4)`, so `` should not be considered as extreme as `4`. You need to use probability space and not count space to assess extremity.

This logic leads to the conclusion that the proper definition is one in which `y` is as extreme or more extreme than `x` if `p(y, n, p) < p(x, n, p)`. This is the correct definition for the exact binomial test. Implementing it leads to this piece of code in Julia for computing p-values for the binomial test:

{% highlight julia %}
load("extras/Rmath.jl")

function binom_p_value(x, n, p)
  sum(filter(d -> d <= dbinom(x, n, p),
             map(i -> dbinom(i, n, p),
                 [0:n])))
end

binom_p_value(2, 10, 0.8)
{% endhighlight %}

As far as I know, this procedure may be as efficient as possible, but it seems odd to me that we should need to assess the PDF at `n + 1` numbers when, in principle, we should only need to assess the CDF of the binomial distribution at two points to find a p-value.

For that reason, you might hope to replace your loop over `n + 1` numbers to one that, for extreme data sets, is more efficient by estimating lower and upper bounds on the count values with lower PDF values than the observed data. For the moment, I'm experimenting with doing this as follows:

{% highlight julia %}
load("extras/Rmath.jl")

function binom_p_value(x, n, p)
  lower_bound = floor(n * p - abs(n * p - x))
  upper_bound = ceil(n * p + abs(n * p - x))
  
  sum(filter(d -> d <= dbinom(x, n, p),
  	     map(i -> dbinom(i, n, p),
	         vcat([0:lower_bound], [upper_bound:n]))))
end

binom_p_value(2, 10, 0.8)
{% endhighlight %}

Unfortunately, I haven't yet done the analytic work to demonstrate that these bounds are actually correct. (One of them must be, since one of them is `a` and the strict monotonicity of the distribution function about `n * p` guarantees that `a` must be either a lower or an upper bound for itself.) Of course, if these bounds are sufficiently conservative, they'll function to save computation without any risk of giving corrupt answers -- even if they're not the tightest possible bounds.

Note that, in principle, it should be possible to go further: if you know exact bounds, then the summing and filtering operations are entirely superfluous and we can run:

{% highlight julia %}
load("extras/Rmath.jl")

function binom_p_value(x, n, p)
  lower_bound = exact_lower_bound(x, n, p)
  upper_bound = exact_upper_bound(x, n, p)
  
  pbinom(lower_bound, n, p) + 1 - pbinom(upper_bound - 1, n, p)
end

binom_p_value(2, 10, 0.8)
{% endhighlight %}

I don't know that these exact bounds can be computed exactly without a lot of work, but if they can be, they give a much more efficient implementation of the exact binomial test.

I wrote all of this up because (a) I'd appreciate knowing if the exact bounds can be computed efficiently and (b) I thought it was a very nice example of (1) thinking through the logic of hypothesis testing in detail (including considerations of what extremity really means) and (2) the constant problem that mathematically equivalent definitions suggest algorithms with very different computational costs.
