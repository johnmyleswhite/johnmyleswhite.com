---
title: The Great Julia RNG Refactor
author: John Myles White
layout: post
permalink: /notebook/2012/06/21/the-great-julia-rng-refactor/
categories:
  - Programming
  - Statistics
---

Many readers of this blog will know that I'm a big fan of Bayesian methods, in large part because automated inference tools like [JAGS](http://mcmc-jags.sourceforge.net/) allow modelers to focus on the types of structure they want to extract from data rather than worry about the algorithmic details of how they will fit their models to data. For me, the ease with which we can construct automated inference tools for Bayesian models is the greatest fruit of the [MCMC Revolution](http://math.uchicago.edu/~shmuel/Network-course-readings/MCMCRev.pdf). And it's embodied in lots of great software packages like [WinBUGS](http://www.mrc-bsu.cam.ac.uk/bugs/winbugs/contents.shtml), [JAGS](http://mcmc-jags.sourceforge.net/), and [PyMC](https://github.com/pymc-devs/pymc) -- among severals others I know less well.

When I develop a new model for analyzing small data sets, I typically can do all of my statistical programming in JAGS. But as soon as the data set I'm working with contains more than 100,000 entries, I find that I need to write sampling code by hand. And, of course, I always write my simulation code by hand.

Traditionally, I would use R for both of these tasks because of the wide variety of RNG's provided by R by default. The convenience of having access to functions like `rbinom()` and `rbeta()` in out-of-the-box R makes it easy to forgive R's faults.

But there's always been a small hole in the RNG's provided by R: there's no `rdirichlet()` function in the "stats" package. In Bayesian statistics, the Dirichlet distribution comes up over and over again, because it is the [conjugate prior](http://en.wikipedia.org/wiki/Conjugate_prior) for models with multinomial data, which include almost all Bayesian models for text analysis. Now, it is true that you can find an R function for Dirichlet random number generation in the "MCMCpack" package, but this is inconvenient and I find that I frequently forget which package contains `rdirichlet()` if I haven't used it recently.

To avoid finding myself in the same situation with Julia, I recently wrote up samplers for the multinomial and Dirichlet distributions. And, as of this week, those samplers are now in the standard branch of Julia.

These new samplers are particularly nice to work with because they can take advantage of a substantial refactor of the Julia RNG design that was worked out by several Julia developers over the past few weeks. I'm going to describe this new interface in general, so that I can show how one can use this interface to draw samples from the multinomial and Dirichlet distributions in Julia.

Inspired by [SciPy](http://docs.scipy.org/doc/scipy/reference/stats.html), the main idea of the new approach to RNG's in Julia is to exploit Julia's multiple dispatch system to make it possible for functions for working with distributions to be named as generically as possible. Instead of using naming conventions as is done in R (think of `rbinom`, `dbinom`, `pbinom` and `qbinom`), Julia uses the exact same function names for all probability distributions. To generate Gaussian, Gamma or Dirichlet draws, you'll always use the `rand()` function. For the probability mass or density functions, you'll always use `pmf()` or `pdf()`. For the cumulative distribution function, you'll always use `cdf()`. This list goes on and on: there are even customized `mean()` and `var()` functions for many distributions that will return the expectation and variance of a theoretical random variable, rather than the empirical mean or variance of a sample from that distribution.

Julia can make this constant naming convention work smoothly by giving each probability distribution its own explicit position in the Julia type system. For example, there is now a `Binomial` type defined as:

{% highlight julia %}
type Binomial
  size::Int
  prob::Float64
end
{% endhighlight %}

To generate a random binomial sample with 10 draws with probability 0.8 of heads for each draw, you therefore run the following code:

{% highlight julia %}
load("base/distributions.jl")

rand(Binomial(10, 0.8))
{% endhighlight %}

I quite like this approach. At times, it makes Julia feel a bit like Mathematica, because you can interact with abstract concepts rather than thinking about data structures, as seen in this calculation of the theoretical variance of a binomial variable:

{% highlight julia %}
load("base/distributions.jl")

var(Binomial(10, 0.8))
{% endhighlight %}

That's enough of an introduction to Julia's new RNG design for me to show off the multinomial and Dirichlet functionality. Here we go:

{% highlight julia %}
load("base/distributions.jl")

##
# Multinomial
##

# The multinomial distribution for one draw of one object with p = [0.5, 0.4, 0.1] of drawing each of three objects.
d = Multinomial(1, [0.5, 0.4, 0.1])

# Draw one object from three types, all equally probable.
d = Multinomial(1, 3)

# Compute the mean and variance of the distribution under its parameterization.
mean(d)
var(d)

# Compute the probability mass function.
pmf(d, [1, 0, 0])
pmf(d, [1, 1, 0])
pmf(d, [0, 1, 0])

# Draw a random sample.
rand(d)

##
# Dirichlet
##

# The Dirichlet distribution with concentration parameter = [1.0, 2.0, 1.0].
d = Dirichlet([1.0, 2.0, 1.0])

# The mean and variance of this distribution.
mean(d)
var(d)

# Compute the probability density function.
pdf(d, [0.1, 0.8, 0.1])

# Draw a random sample.
rand(d)
{% endhighlight %}

I'm excited to have these tools in Julia by default now. They'll make it much easier to write clean code for Gibbs samplers for models like LDA that make heavy use of Dirichlet priors.

I also want to note that while I was writing up these new samplers, Chris DuBois has been building some very general purpose MCMC functions that will let people write a sampler for an arbitrary Bayesian model fairly easily. You can see his examples on [GitHub](https://github.com/doobwa/mcmc.jl).
