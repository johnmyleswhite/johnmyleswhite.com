---
title: The FourierDescriptors Package
author: John Myles White
layout: post
permalink: /notebook/2010/09/04/the-fourierdescriptors-package/
categories:
  - Statistics
---

### Introduction

I've just uploaded a new package to CRAN based on a stimulus generation algorithm that I use for my experiments on vision. The FourierDescriptors package provides methods for creating, manipulating and visualizing Fourier descriptors, which are a representational scheme used to describe closed planar contours. The canonical reference from the literature is [Zahn and Roskies 1972](http://www.cs.jhu.edu/~misha/Papers/Zahn72.pdf). The images most easily described using Fourier descriptors are useful as stimuli for experiments in psychology and neuroscience.

### Installation

This package has been submitted to CRAN. When it propagates through the mirrors, you can install it using a simple call to `install.packages()`:

{% highlight r %}
install.packages('FourierDescriptors')
{% endhighlight %}

For the time being, please install it using the included source package by downloading the [GitHub repository](http://github.com/johnmyleswhite/FourierDescriptors) and running:

{% highlight bash %}
R CMD INSTALL FourierDescriptors_*.tar.gz
{% endhighlight %}

### Basic Usage Example

The following example showcases the central methods that currently exist in the FourierDescriptors package:

{% highlight r %}
library('FourierDescriptors')

fd1 <- create.fourier.descriptor()
print(fd1)
plot(fd1)

fd2 <- random.fourier.descriptor(32, 2)
plot(fd2)
{% endhighlight %}

### Gaining Intuition about the Amplitude Spectrum

It's useful to see the images generated by very basic amplitude spectra to see how these determine the shape of the induced contour.

{% highlight r %}
library('FourierDescriptors')
plot(create.fourier.descriptor(amplitude = c(0, 0, 0, 0)))
{% endhighlight %}

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2010/09/image1.png" alt="image1.png" />
</center>

{% highlight r %}
plot(create.fourier.descriptor(amplitude = c(0, 1, 0, 0)))
{% endhighlight %}

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2010/09/image2.png" alt="image2.png" />
</center>

{% highlight r %}
plot(create.fourier.descriptor(amplitude = c(0, 0, 0, 1)))
{% endhighlight %}

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2010/09/image3.png" alt="image3.png" />
</center>

{% highlight r %}
plot(create.fourier.descriptor(amplitude = c(0, 1, 0, 1)))
{% endhighlight %}

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2010/09/image4.png" alt="image4.png" />
</center>

{% highlight r %}
plot(random.fourier.descriptor(24,
                               non.zero.frequencies = 4,
                               generating.function = function() {runif(1)}),
     steps = 360 * 10)
{% endhighlight %}

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2010/09/image5.png" alt="image5.png" />
</center>

**Please note that only even-numbered frequencies can have non-zero amplitudes or the described curve will not be closed.**