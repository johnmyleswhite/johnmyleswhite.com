---
title: Using Norms to Understand Linear Regression
author: John Myles White
layout: post
permalink: /notebook/2013/03/22/using-norms-to-understand-linear-regression/
categories:
  - Statistics
---

### Introduction

In [my last post](http://www.johnmyleswhite.com/notebook/2013/03/22/modes-medians-and-means-an-unifying-perspective/), I described how we can derive modes, medians and means as three natural solutions to the problem of summarizing a list of numbers, $(x\_1, x\_2, \ldots, x\_n)$, using a single number, $s$. In particular, we measured the quality of different potential summaries in three different ways, which led us to modes, medians and means respectively. Each of these quantities emerged from measuring the typical discrepancy between an element of the list, $x\_i$, and the summary, $s$, using a formula of the form,
<div class="well">
$$
\sum_i |x_i - s|^p,
$$
</div>
where $p$ was either $0$, $1$ or $2$.

### The $L_p$ Norms

In this post, I'd like to extend this approach to linear regression. The notion of discrepancies we used in the last post is very closely tied to the idea of measuring the size of a vector in $\mathbb{R}^n$. Specifically, we were minimizing a measure of discrepancies that was almost identical to the $L\_p$ family of norms that can be used to measure the size of vectors. Understanding $L\_p$ norms makes it much easier to describe several modern generalizations of classical linear regression.

To extend our previous approach to the more standard notion of an $L\_p$ norm, we simply take the sum we used before and rescale things by taking a $p^{th}$ root. This gives the formula for the $L\_p$ norm of any vector, $v = (v\_1, v\_2, \ldots, v_n)$, as,  
<div class="well">
$$  
|v|_p = (\sum_i |v_i|^p)^\frac{1}{p}.  
$$  
</div>
When $p = 2$, this formula reduces to the familiar formula for the length of a vector:  
<div class="well">
$$  
|v|_2 = \sqrt{\sum_i v_i^2}.  
$$
</div>

In the last post, the vector we cared about was the vector of elementwise discrepancies, $v = (x\_1 - s, x\_2 - s, \ldots, x\_n - s)$. We wanted to minimize the overall size of this vector in order to make $s$ a good summary of $x\_1, \ldots, x\_n$. Because we were interested only in the minimum size of this vector, it didn't matter that we skipped taking the $p^{th}$ root at the end because one vector, $v\_1$, has a smaller norm than another vector, $v\_2$, only when the $p^{th}$ power of that norm smaller than the $p^{th}$ power of the other. What was essential wasn't the scale of the norm, but rather the value of $p$ that we chose. Here we'll follow that approach again. Specifically, we'll again be working consistently with the $p^{th}$ power of an $L\_p$ norm:  
<div class="well">
$$  
|v|_p^p = (\sum_i |v_i|^p).  
$$
</div>

### The Regression Problem

Using $L_p$ norms to measure the overall size of a vector of discrepancies extends naturally to other problems in statistics. In the previous post, we were trying to summarize a list of numbers by producing a simple summary statistic. In this post, we're instead going to summarize the relationship between two lists of numbers in a form that generalizes traditional regression models.

Instead of a single list, we'll now work with two vectors: $(x\_1, x\_2, \ldots, x\_n)$ and $(y\_1, y\_2, \ldots, y\_n)$. Because we like simple models, we'll make the very strong (and very convenient) assumption that the second vector is, approximately, a linear function of the first vector, which gives us the formula:  
<div class="well">
$$  
y_i \approx \beta_0 + \beta_1 x_i.  
$$
</div>
In practice, this linear relationship is never perfect, but only an approximation. As such, for any specific values we choose for $\beta\_0$ and $\beta\_1$, we have to compute a vector of discrepancies: $v = (y\_1 - (\beta\_0 + \beta\_1 x\_1), \ldots, y\_n - (\beta\_0 + \beta\_1 x\_n))$. The question then becomes: how do we measure the size of this vector of discrepancies? By choosing different norms to measure its size, we arrive at several different forms of linear regression models. In particular, we'll work with three norms: the $L\_0$, $L\_1$ and $L_2$ norms.

As we did with the single vector case, here we'll define discrepancies as,  
<div class="well">
$$  
d_i = |y_i - (\beta_0 + \beta_1 x_i)|^p,  
$$  
</div>
and the total error as,  
<div class="well">
$$  
E_p = \sum_i |y_i - (\beta_0 + \beta_1 x_i)|^p,  
$$  
</div>
which is the just the $p^{th}$ power of the $L_p$ norm.

### Several Forms of Regression

In general, we want estimate a set of regression coefficients that minimize this total error. Different forms of linear regression appear when we alter the values of $p$. As before, let's consider three settings:  
<div class="well">
$$  
E_0 = \sum_i |y_i - (\beta_0 + \beta_1 x_i)|^0  
$$  
</div>

<div class="well">
$$  
E_1 = \sum_i |y_i - (\beta_0 + \beta_1 x_i)|^1  
$$  
</div>

<div class="well">
$$  
E_2 = \sum_i |y_i - (\beta_0 + \beta_1 x_i)|^2  
$$
</div>

What happens in these settings? In the first case, we select regression coefficients so that the line passes through as many points as possible. Clearly we can always select a line that passes through any pair of points. And we can show that there are data sets in which we cannot do better. So the $L_0$ norm doesn't seem to provide a very useful form of linear regression, but I'd be interested to see examples of its use.

In contrast, minimizing $E\_1$ and $E\_2$ define quite interesting and familiar forms of linear regression. We'll start with $E\_2$ because it's the most familiar: it defines Ordinary Least Squares (OLS) regression, which is the one we all know and love. In the $L\_2$ case, we select $\beta\_0$ and $\beta\_1$ to minimize,  
<div class="well">
$$  
E_2 = \sum_i (y_i - (\beta_0 + \beta_1 x_i))^2,  
$$  
</div>
which is the summed squared error over all of the $(x\_i, y\_i)$ pairs. In other words, Ordinary Least Squares regression is just an attempt to find an approximating linear relationship between two vectors that minimizes the $L_2$ norm of the vector of discrepancies.

Although OLS regression is clearly king, the coefficients we get from minimizing $E\_1$ are also quite widely used: using the $L\_1$ norm defines Least Absolute Deviations (LAD) regression, which is also sometimes called Robust Regression. This approach to regression is robust because large outliers that would produce errors greater than $1$ are not unnecessarily augmented by the squaring operation that's used in defining OLS regression, but instead only have their absolute values taken. This means that the resulting model will try to match the overall linear pattern in the data even when there are some very large outliers.

We can also relate these two approaches to the strategy employed in the previous post. When we use OLS regression (which would be better called $L\_2$ regression), we predict the mean of $y\_i$ given the value of $x\_i$. And when we use LAD regression (which would be better called $L\_1$ regression), we predict the median of $y\_i$ given the value of $x\_i$. Just as I said in the previous post, the core theoretical tool that we need to understand is the $L_p$ norm. For single number summaries, it naturally leads to modes, medians and means. For simple regression problems, it naturally leads to LAD regression and OLS regression. But there's more: it also leads naturally to the two most popular forms of regularized regression.

### Regularization

If you're not familiar with regularization, the central idea is that we don't exclusively try to find the values of $\beta\_0$ and $\beta\_1$ that minimize the discrepancy between $\beta\_0 + \beta\_1 x\_i$ and $y\_i$, but also simultaneously try to satisfy a competing requirement that $\beta\_1$ not get too large. Note that we don't try to control the size of $\beta\_0$ because it describes the overall scale of the data rather than the relationship between $x$ and $y$.

Because these objectives compete, we have to combine them into a single objective. We do that by working with a linear sum of the two objectives. And because both the discrepancy objective and the size of the coefficients can be described in terms of norms, we'll assume that we want to minimize the $L\_p$ norm of the discrepancies and the $L\_q$ norm of the $\beta$'s. This means that we end up trying to minimize an expression of the form,  
<div class="well">
$$  
(\sum_i |y_i - (\beta_0 + \beta_1 x_i)|^{p}) + \lambda (|\beta_1|^q).  
$$
</div>

In most regularized regression models that I've seen in the wild, people tend to use $p = 2$ and $q = 1$ or $q = 2$. When $q = 1$, this model is called the LASSO. When $q = 2$, this model is called ridge regression. In another approach, I'll try to describe why the LASSO and ridge regression produce such different patterns of coefficients.
