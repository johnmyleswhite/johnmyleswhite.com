---
title: Automatic Hyperparameter Tuning Methods
author: John Myles White
layout: post
permalink: /notebook/2012/07/21/automatic-hyperparameter-tuning-methods/
categories:
  - Programming
  - Statistics
---

At MSR this week, we had two very good talks on algorithmic methods for tuning the hyperparameters of machine learning models. Selecting appropriate settings for hyperparameters is a constant problem in machine learning, which is somewhat surprising given how much expertise the machine learning community has in optimization theory. I suspect there's interesting psychological and sociological work to be done exploring why a problem that could be answered using known techniques wasn't given an appropriate solution earlier.

Thankfully, the take away message of this blog post is that this problem is starting to be understood.

### A Two-Part Optimization Problem

To set up the problem of hyperparameter tuning, it's helpful to think of the canonical model-tuning and model-testing setup used in machine learning: one splits the original data set into three parts -- a training set, a validation set and a test set. If, for example, we plan to use L2-regularized linear regression to solve our problem, we will use the training set and validation set to select a value for the $\lambda$ hyperparameter that is used to determine the strength of the penalty for large coefficients relative to the penalty for errors in predictions.

With this context in mind, we can set up our problem using five types of variables:

* Features: $x$
* Labels: $y$
* Parameters: $\theta$
* Hyperparameters: $\lambda$
* Cost function: $C$

We then estimate our parameters and hyperparameters in the following multi-step way so as to minimize our cost function:

<div class="well">
$$
\theta_{Train}(\lambda) = \arg \min_{\theta} C(x_{Train}, y_{Train}, \theta, \lambda)  
$$
</div>

<div class="well">
$$
\lambda_{Validation}^{*} = \arg \min_{\lambda} C(x_{Validation}, y_{Validation}, \theta_{Train}(\lambda), \lambda)  
$$
</div>

The final model performance is assessed using:  
<div class="well">
$$
C(x_{Test}, y_{Test}, \theta_{Train + Validation}(\lambda_{Validation}^{*}), \lambda_{Validation}^{*})  
$$
</div>

This two-part minimization problem is similar in many ways to stepwise regression. Like stepwise regression, it feels like an opportunity for clean abstraction is being passed over, but it's not clear to me (or anyone I think) if there is any analytic way to solve this problem more abstractly.

Instead, the methods we saw presented in our seminars were ways to find better approximations to $\lambda^{*}$ using less compute time. I'll go through the traditional approach, then describe the newer and cleaner methods.

### Grid Search

Typically, hyperparameters are set using the [Grid Search](http://en.wikipedia.org/wiki/Grid_search) algorithm, which works as follows:

* For each parameter $p\_{i}$ the researcher selects a list of values to test empirically.
* For each element of the Cartesian product of these values, the computer evaluates the cost function.
* The computer selects the hyperparameter settings from this grid with the lowest cost.

Grid Search is about the worst algorithm one could possibly use, but it's in widespread use because (A) machine learning experts seem to have less familiarity with derivative-free optimization techniques than with gradient-based optimization methods and (B) machine learning culture does not traditionally think of hyperparameter tuning as a formal optimization problem. Almost certainly (B) is more important than (A).

### Random Search

James Bergstra's first proposed solution was so entertaining because, absent evidence that it works, it seems almost flippant to even propose: he suggested replacing Grid Search with [Random Search](http://jmlr.csail.mit.edu/papers/v13/bergstra12a.html). Instead of selecting a grid of values and walking through it exhaustively, you select a value for each hyperparameter independently using some probability distribution. You then evaluate the cost function given these random settings for the hyperparameters.

Since this approach seems like it might be worst than Grid Search, it's worth pondering why it should work. James' argument is this: most ML models have *low-effective dimension*, which means that a small number of parameters really affect the cost function and most have almost no effect. Random search lets you explore a greater variety of settings for each parameter, which allows you to find better values for the few parameters that really matter.

I am sure that [Paul Meehl](http://www.futurepundit.com/archives/001558.html) would have a field day with this research if he were alive to hear about it.

### Arbitrary Regression Problem

An alternative approach is to view our problem as one of [Bayesian Optimization](http://www.cs.ubc.ca/~hutter/nips2011workshop/index.html): we have an arbitrary function that we want to minimize which is costly to evaluate and we would like to find a good approximate minimum in a small number of evaluations.

When viewed in this perspective, the natural strategy is to regress the cost function on the settings of the hyperparameters. Because the cost function may depend on the hyperparameters in strange ways, it is wise to use very general purpose regression methods. I've recently seen two clever strategies for this, one of which was presented to us at MSR:

* Jasper Snoek, Hugo Larochelle and Ryan Adams suggest that one use a [Gaussian Process](http://arxiv.org/pdf/1206.2944.pdf).
* Among other methods, [Frank Hutter, Holger H. Hoos and Kevin Leyton-Brown](http://www.cs.ubc.ca/~hutter/papers/11-LION5-SMAC.pdf) suggest that one use Random Forests.

From my viewpoint, it seems that any derivative-free optimization method might be worth trying. While I have yet to see it published, I'd like to see more people try the [Nelder-Mead method](http://en.wikipedia.org/wiki/Nelder–Mead_method) for tuning hyperparameters.
