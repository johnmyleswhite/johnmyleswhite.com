---
title: Pearson vs. Spearman Correlation Coefficients
author: John Myles White
layout: post
permalink: /notebook/2009/02/17/pearson-vs-spearman-correlation-coefficients/
categories:
  - Statistics
---

One of the misuses of statistical terminology that annoys me most is the use of the word "correlation" to describe any variable that increases as another variable increases. This monotonic trend seems worth looking for, but it plainly is not what most people discover when they use standard correlation coefficients. This is because the Pearson product moment correlation coefficient, which is usually the only correlation coefficient students learn to calculate, is strongly biased towards linear trends: those in which a variable y is a noisy linear function of a variable x. Only the Spearman correlation coefficient, which is usually not taught to students, actually detects a general monotonic trend. You can see this for yourself easily by seeing what the correlation coefficient is between **x** and progressively higher-degree polynomials in **x**.

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2009/02/pearson-vs-spearman.png" alt="Pearson vs Spearman.png" />
</center>

If the Pearson correlation coefficient actually detected monotonic trends, it wouldn't plunge to zero as the degree of the polynomial in x increases. This is precisely what the Spearman correlation coefficient does.

I hope that we can reconcile our intuitive thinking and our statistical practice by ending the self-contradiction in which the word "correlation" is used in discourse to describe the behavior of an ideal Spearman correlation coefficient, while in practice correlations are computed using Pearson's formula.
