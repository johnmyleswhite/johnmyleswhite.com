---
title: Quick Review of Matrix Algebra in R
author: John Myles White
layout: post
permalink: /notebook/2009/12/16/quick-review-of-matrix-algebra-in-r/
categories:
  - Observations
  - Statistics
---

Lately, I've been running a series of fMRI experiments on visual perception. In the interests of understanding the underlying properties of the images I'm using as stimuli, I've been trying to learn more about the matrix transformations commonly used for image compression and image manipulation. Thankfully, R provides simple-to-use implementations for all of the matrix operations I wanted to play around with, so it's been quite easy to get started. For the next few posts, I thought that I'd review the standard matrix techniques for image compression and editing, giving full examples of their implementation in R and demonstrations of their real-world value.

Before I start, I should make sure that you're familiar with basic matrix operations in R. I imagine almost every R user knows a little bit about matrix algebra and probably knows the basics of using R to perform matrix algebra, but here's a quick review to make sure I don't leave anyone in the dark:

### Building Matrices

You can build a matrix in R using the `matrix` function:

{% highlight r %}
m <- matrix(c(1, 0, 0, 1), nrow = 2, ncol = 2, byrow = TRUE)

m
#      [,1] [,2]
# [1,]    1    0
# [2,]    0    1
{% endhighlight %}

You can test whether an item you've been given is a matrix using `is.matrix` and you can convert appropriate objects to matrices using `as.matrix`:

{% highlight r %}
m <- matrix(c(1, 0, 0, 1), nrow = 2, ncol = 2, byrow = TRUE)

is.matrix(m)
# TRUE

as.matrix(data.frame(x = c(1, 0), y = c(0, 1)))
#      x y
# [1,] 1 0
# [2,] 0 1
{% endhighlight %}

### Diagonal Matrices

The matrix I've been building in the examples above is a diagonal matrix, so you could also construct it using `diag`:

{% highlight r %}
m <- diag(nrow = 2, ncol = 2)

m
#      [,1] [,2]
# [1,]    1    0
# [2,]    0    1
{% endhighlight %}

In general, you can generate the n by n identity matrix as:

{% highlight r %}
n <- 10

diag(nrow = n, ncol = n)
{% endhighlight %}

I'll be exploiting a more interesting use of `diag` in my next post, so let's see how you can build matrices other than the identity with `diag` by specifying a vector of entries along the diagonal:

{% highlight r %}
m <- diag(c(2, 1), nrow = 2, ncol = 2)

m
#      [,1] [,2]
# [1,]    2    0
# [2,]    0    1
{% endhighlight %}

This form of `diag` turns out to be extremely useful, as you'll see once I cover the SVD's syntax in R.

### Matrix Algebra: Addition, Scalar Multiplication, Matrix Multiplication

The three core operations that can be performed on matrices are addition, scalar multiplication and matrix multiplication. These are easy to work with in R:

{% highlight r %}
m <- matrix(c(0, 2, 1, 0), nrow = 2, ncol = 2, byrow = TRUE)

m + m
#      [,1] [,2]
# [1,]    0    4
# [2,]    2    0

2 * m
#      [,1] [,2]
# [1,]    0    4
# [2,]    2    0

m %*% m
#      [,1] [,2]
# [1,]    2    0
# [2,]    0    2
{% endhighlight %}

Be careful with the `*` operator: it does not perform matrix multiplication, but rather an entry-wise multiplication:

{% highlight r %}
m * m

#      [,1] [,2]
# [1,]    0    4
# [2,]    1    0
{% endhighlight %}

### Matrix Transposes and Inverses

The next few matrix operations are a little more complex: transposition and inversion. Transposition is the easier of the two. To get the transpose of a matrix, you simply call the `t` function:

{% highlight r %}
t(m)
#      [,1] [,2]
# [1,]    0    1
# [2,]    2    0
{% endhighlight %}

In contrast, inversion is a little more complex, partly because the function you'd want to use has a non-obvious name: `solve`.

{% highlight r %}
solve(m)

#      [,1] [,2]
# [1,]  0.0    1
# [2,]  0.5    0
{% endhighlight %}

The reason that `solve` is called `solve` is that it's a general purpose function you can use to solve matrix equations without wasting time computing the full inverse, which is often inefficient. If you want to know more about the computational efficiency issues, you should look into the ideas behind the even faster variant, `qr.solve`.

Now, you probably know this already, but the definition of a matrix's inverse is that the product of the matrix and its inverse is the identity matrix, if the inverse exists. I always find this a good way to make sure that I'm correctly computing the inverse of a matrix:

{% highlight r %}
solve(m) %*% m == diag(nrow = nrow(m), ncol = ncol(m))

#      [,1] [,2]
# [1,] TRUE TRUE
# [2,] TRUE TRUE
{% endhighlight %}

### Further Matrix Operations

The `car` package defines an `inv` function, which is simply a new name for `solve`:

{% highlight r %}
library('car')
inv

# function (x) 
# solve(x)
# <environment: namespace:car>
{% endhighlight %}

I think this is pretty clever, though I'm loathe to import a package just for this one snippet.

More interestingly, the `MASS` package defines a `ginv` function, which gives the matrix pseudoinverse, a generalization of matrix inversion that works for all matrices:

{% highlight r %}
library('MASS')

m <- matrix(c(1, 1, 2, 2), nrow = 2, ncol = 2)

solve(m)
# Error in solve.default(m) : 
#  Lapack routine dgesv: system is exactly singular

ginv(m)
#      [,1] [,2]
# [1,]  0.1  0.1
# [2,]  0.2  0.2
{% endhighlight %}

In practice, you can usually get around using the pseudoinverse, but it's nice to know that it's at hand all the time. 

### Eigenvalues and Eigenvectors

Eigenvectors are surely the bane of every starting student of linear algebra, though their considerable power to simplify problems makes them the darling of every applied mathematician. Thankfully, R makes it easy to get these for every matrix:

{% highlight r %}
m <- diag(nrow = 2, ncol = 2)

eigen(m)
# $values
# [1] 1 1
#
# $vectors
#      [,1] [,2]
# [1,]    0   -1
# [2,]    1    0
{% endhighlight %}

### Matrix Metadata

Last, but not least, you can get metadata about the shape of a matrix using `dim`, `nrow` and `ncol`:

{% highlight r %}
m <- diag(nrow = 2, ncol = 2)

dim(m)
# [1] 2 2

nrow(m)
# [1] 2

ncol(m)
# [1] 2
{% endhighlight %}

Hopefully those operations were already familiar to any readers out there, as I doubt that they'll be clear from this short explanation without prior knowledge. I just felt compelled to review them before using any of them in my next set of posts. Tomorrow, I'll start going through more interesting matrix algorithms in R, beginning with the SVD.
