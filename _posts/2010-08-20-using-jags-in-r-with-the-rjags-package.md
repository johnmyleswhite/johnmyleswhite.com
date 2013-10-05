---
title: Using JAGS in R with the rjags Package
author: John Myles White
layout: post
permalink: /notebook/2010/08/20/using-jags-in-r-with-the-rjags-package/
categories:
  - Statistics
---

### Get Everything Set Up

I'm going to assume that you have access to a machine that will run JAGS. If you don't, then you should be able to use [WinBUGS](http://www.mrc-bsu.cam.ac.uk/bugs/), which is very easy to get set up. Unfortunately, the details of what follows may not help you as much if you're using WinBUGS.

To set up your system for using JAGS, there are two very easy steps:

* Go [download the current version](http://www-fis.iarc.fr/~martyn/software/jags/) of JAGS (2.1.0 as of 8/20/2010).
* Install the current [rjags](http://cran.r-project.org/web/packages/rjags/index.html) package from CRAN (2.1.0-6 as of 8/20/2010).

Once you've done that, a simple call to `library('rjags')` will be enough to run JAGS from inside of R. You'll want to do everything except model specification in R. You'll specify the model in a separate file using BUGS/JAGS syntax.

### Example 1: Inference on Normally Distributed Data

Let's assume that you've got a bunch of data points from a normal distribution with unknown mean and variance. This is arguably the simplest data set you can analyze with JAGS. So, how do you perform the analysis?

First, let's create some simulation data that we'll use to test our JAGS model specification:

{% highlight r %}
N <- 1000
x <- rnorm(N, 0, 5)

write.table(x,
            file = 'example1.data',
            row.names = FALSE,
            col.names = FALSE)
{% endhighlight %}

We don't actually need to write out the data since 'rjags' automatically does this for us (and in another format at that), but it's nice to be able to check that JAGS has done something reasonable by analyzing the raw inputs post hoc.

With your simulated data in hand, we'll write up a model specification in JAGS syntax. Put the model specification in a file called `example1.bug`. The complete model looks like this:

{% highlight r %}
model {
	for (i in 1:N) {
		x[i] ~ dnorm(mu, tau)
	}
	mu ~ dnorm(0, .0001)
	tau <- pow(sigma, -2)
	sigma ~ dunif(0, 100)
}
{% endhighlight %}

In every model specification file, you have to start out by telling JAGS that you're specifying a model. Then you set up the model for every single data point using a `for` loop. Here, we say that `x[i]` is distributed normally (hence the `dnorm()` call) with mean `mu` and precision `tau`, where the precision is simply the reciprocal of the variance. Then we specify our priors for `mu` and `tau`, which are meant to be constant across the loop. We tell JAGS that `mu` is distributed normally with mean 0 and standard deviation 100. This is meant to serve as a non-informative prior, since our data set was designed to have all measurements substantially below 100. Then we specify `tau` in a slightly round-about way. We say that `tau` is a deterministic function (hence the deterministic `<-` instead of the distributional `~`) of `sigma`, after raising `sigma` to the -2 power. Then we say that `sigma` has a uniform prior over the interval [0,100].

With this model specified in `example1.bug`, we can write more R code to invoke it and perform inference properly:

{% highlight r %}
library('rjags')

jags <- jags.model('example1.bug',
                   data = list('x' = x,
                               'N' = N),
                   n.chains = 4,
                   n.adapt = 100)

update(jags, 1000)

jags.samples(jags,
             c('mu', 'tau'),
             1000)
{% endhighlight %}

Obviously, we have to import the 'rjags' package. Then we need to set up our model object in R, which we do using the `jags.model()` function. We specify the JAGS model specification file and the data set, which is a named list where the names must be those used in the JAGS model specification file. Finally, we tell the system how many parallel chains to run. (If you don't understand what the chains represent, I'd suggest just playing around and then reading up about the issue of mixing in MCMC.) Finally, we tell the system how many samples should be thrown away as part of the adaptive sampling period for each chain. For this example, I suspect that we could safely set this parameter to 0, but it costs so little that I've used 100 just as a placeholder. After calling `jags.model()`, we receive a JAGS model object, which we store in the `jags` variable.

After all of that set up, I've chosen to have the system run another 1000 iterations of the sampler just to show how to use the `update()` function, even though it's completely unnecessary in this simple problem. Finally, we use `jags.sample()` to draw 1000 samples from the sampler for the values of the named variables `mu` and `tau`.

When you call `jags.sample()`, you'll see the output provides proposed values for `mu` and `tau`. These should be close to 0 and 0.04 if JAGS is working properly, since those were the mean and precision values we used to create our simulation data. (At the risk of being pedantic: we used a standard deviation of 5, which gives a variance of 25 and a precision of 1 / 25 = 0.04.) Of course, they'll be even closer to the sample mean `mean(x)` and the sample precision `1 / var(x)`, so you should not forget to compare the inferred values to these values. The sample size, 1,000, isn't large enough to guarantee that the mean will be all that close to 0.

### Example 2: Basic Linear Regression

Moving on to a slightly more interesting example, we can perform a simple linear regression in JAGS very easily. As before, we set up simulation data from a theoretical linear model:

{% highlight r %}
N <- 1000
x <- 1:N
epsilon <- rnorm(N, 0, 1)
y <- x + epsilon

write.table(data.frame(X = x, Y = y, Epsilon = epsilon),
            file = 'example2.data',
            row.names = FALSE,
            col.names = TRUE)
{% endhighlight %}

We then set up the Bayesian model for our regression in `example2.bug`:

{% highlight r %}
model {
	for (i in 1:N){
		y[i] ~ dnorm(y.hat[i], tau)
		y.hat[i] <- a + b * x[i]
	}
	a ~ dnorm(0, .0001)
	b ~ dnorm(0, .0001)
	tau <- pow(sigma, -2)
	sigma ~ dunif(0, 100)
}
{% endhighlight %}

Here, we've said that every data point is drawn from a normal distribution with mean `a + b * x[i]` and precision `tau`. We assign non-informative normal priors to `a` and `b` and a non-informative uniform prior to the standard deviation `sigma`, which is deterministically transformed into `tau`.

Then, we run this model using the same exact approach as we used earlier:

{% highlight r %}
library('rjags')

jags <- jags.model('example2.bug',
                   data = list('x' = x,
                               'y' = y,
                               'N' = N),
                   n.chains = 4,
                   n.adapt = 100)

update(jags, 1000)

jags.samples(jags,
             c('a', 'b'),
             1000)
{% endhighlight %}

After running the chain for a good number of samples, we draw inferences for `a` and `b`, which should be close to the proper values of 0 and 1. I've ignored `tau` here, though there's no reason not to check that it was properly inferred.

### Example 3: One Dimensional Logistic Regression

Finally, it's good to see a model that's harder to implement without a good deal of knowledge of optimization tools unless you use a sampling technique like the one JAGS automates. For that purpose, I'll show how to implement logistic regression. Here we set up a simple one-dimensional predictor for our binary outcome variable and assume the standard logistic model:

{% highlight r %}
N <- 1000
x <- 1:N
z <- 0.01 * x - 5
y <- sapply(1 / (1 + exp(-z)), function(p) {rbinom(1, 1, p)})

write.table(data.frame(X = x, Z = z, Y = y),
            file = 'example3.data',
            row.names = FALSE,
            col.names = TRUE)
{% endhighlight %}

Then we set up our Bayesian model in `example3.bug`, where `y[i]` is Bernoulli distributed (or binomial distributed with 1 draw, if you prefer that sort of thing) and the linear model coefficients `a` and `b` are given non-informative normal priors:

{% highlight r %}
model {
	for (i in 1:N){
		y[i] ~ dbern(p[i])
		p[i] <- 1 / (1 + exp(-z[i]))
		z[i] <- a + b * x[i]
	}
	a ~ dnorm(0, .0001)
	b ~ dnorm(0, .0001)
}
{% endhighlight %}

Finally, we perform our standard inference calls in R to run the model through JAGS and extract predicted values for `a` and `b`. 

{% highlight r %}
library('rjags')

jags <- jags.model('example3.bug',
                   data = list('x' = x,
                               'y' = y,
                               'N' = N),
                   n.chains = 4,
                   n.adapt = 100)

update(jags, 1000)

jags.samples(jags,
             c('a', 'b'),
             1000)
{% endhighlight %}

As always, you should check that the outputs you get make sense: here, you expect `a` to be approximately -5 and `b` to be around 0.01. This inference problem should take a good bit longer to solve: there are other tools for handling logistic regressions in JAGS that are faster, but I find this approach conceptually simplest and best for highlighting the similarity to a standard linear regression.

### Some Errors I Made at the Start

Here are a few errors that stumped me for a bit as I got started using JAGS today:

* **Error 1, 'Invalid parent error'**: I got this error when I erroneously assigned a normal prior to one of my precision variables `tau`. This is nonsensical, since precisions are always positive. Hence, the parent node involving `tau` was deemed to be 'invalid', causing a fatal run-time error.
* **Error 2, 'Attempt to redefine node z[1]'**: This is an error that Gelman and Hill warn users about in their [ARM book](http://www.stat.columbia.edu/~gelman/arm/): you must be sure that you don't treat the values in loops as local variables, because they cannot be reset on each iteration -- they must have unique values across all iterations. Thus, you must build an array for all of the variables that you might think of as local within loops, such as intermediate latent variables. Not doing so will produce fatal run-time errors.
* **Error 3, 'Invalid vector argument to exp'**: This is related to the error above: when I corrected part of my attempt to reset `z` on each pass through the logistic loop in Example 3, I forgot to reset it in the definition of `p[i]`. This led to an invalid vector with no definition being passed to the `exp()` function, giving a fatal run-time error.

### One Question I Have

My first attempt to run a linear regression didn't work. I still don't entirely understand why, but here is the alternative code that failed if you ever make the same type of mistake and find yourself puzzled:

{% highlight r %}
model {
	for (i in 1:N){
		y[i] <- a + b * x[i] + epsilon[i]
                epsilon[i] ~ dnorm(0, tau)
	}
	a ~ dnorm(0, .0001)
	b ~ dnorm(0, .0001)
	tau <- pow(sigma, -2)
	sigma ~ dunif(0, 100)
}
{% endhighlight %}

My assumption is that it's more difficult to infer values for all the `epsilon`'s at the same time as `tau`, which makes this harder than the earlier call without any explicit `epsilon` values. If that's wrong, please do correct me. Another hypothesis I entertained is that it's a problem to ever set `y[i]` to be a deterministic node, though this doesn't seem really plausible to me.
