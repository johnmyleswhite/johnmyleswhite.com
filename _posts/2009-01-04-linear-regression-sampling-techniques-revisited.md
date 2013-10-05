---
title: Linear Regression Sampling Techniques Revisited
author: John Myles White
layout: post
permalink: /notebook/2009/01/04/linear-regression-sampling-techniques-revisited/
categories:
  - Statistics
---

While thinking about linear regression today, I believe that I've realized why using clustered sampling and two point average slope calculation works better than least squares regression with scattered sampling. Specifically, it is the least squares formula that itself causes the problem, because squaring the errors gives undue weight to certain errors, skewing the results. This skew quickly goes away as the sample size grows, but in small samples it can make a large difference.

It will probably take me another few weeks before I can find time to prove this mathematically.
