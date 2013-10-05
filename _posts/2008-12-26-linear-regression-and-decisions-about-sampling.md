---
title: Linear Regression and Decisions about Sampling
author: John Myles White
layout: post
permalink: /notebook/2008/12/26/linear-regression-and-decisions-about-sampling/
categories:
  - Observations
---

Lately I've been thinking about the optimal strategy for data collection when you plan to run a linear regression. Clearly, you want a sample of widely distributed points if you're unsure that a strict linearity assumption is appropriate. If you already know from theoretical reasons that linearity is appropriate, then you know that you only need two correct $(x, y)$ data points to uniquely define the regression line. To get this, one conventionally samples many $(x, y)$ pairs and then computes the regression line's slope and intercept. Why not sample only two x data points over and over again instead? If you are trying to find the formula for the line $\mathbb{E}[y | x]$, it seems reasonable to assume that high quality estimates of the points $(a, \mathbb{E}[y | x = a])$ and $(b, \mathbb{E}[y | x = b])$ would be a good way to do this.

Is this a reasonable approach to two variable linear regressions? Is this approach less efficient statistically than sampling at many points? Or is the reason to avoid this strategy in practice is that one is uncertain of the validity of the linearity assumption in all but exceptional cases?
