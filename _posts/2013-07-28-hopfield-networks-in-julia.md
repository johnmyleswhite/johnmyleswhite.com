---
title: Hopfield Networks in Julia
author: John Myles White
layout: post
permalink: /notebook/2013/07/28/hopfield-networks-in-julia/
categories:
  - Programming
  - Statistics
---

As a fun side project last night, I decided to implement a basic package for working with [Hopfield networks in Julia](https://github.com/johnmyleswhite/HopfieldNets.jl).

Since I suspect many of the readers of this blog have never seen a Hopfield net before, let me explain what they are and what they can be used for. The short-and-skinny is that Hopfield networks were invented in the 1980's to demonstrate how a network of simple neurons might learn to associate incoming stimuli with a fixed pool of existing memories. As you'll see from the examples below, this associative ability behaves a little bit like [locality-sensitive hashing](http://en.wikipedia.org/wiki/Locality-sensitive_hashing).

To see how Hopfield networks work, we need to define their internal structure. For the purposes of this blog post, we'll assume that a Hopfield network is made up of N neurons. At every point in time, this network of neurons has a simple binary state, which I'll associate with a vector of -1's and +1's.

Incoming stimuli are also represented using binary vectors of length N. Every time one of these stimuli is shown to the network, the network will use a simple updating rule to modify its state. The network will keep modifying its state until it settles into a stable state, which will be one of many fixed points for the updating rule. We'll refer to the stable state that the network reaches as the memory that the network associates with the input stimulus.

For example, let's assume that we have a network consisting of 42 neurons arranged in a 7x6 matrix. We'll train our network to recognize the letters X and O, which will also be represented as 7x6 matrices. After training the network, we'll present corrupted copies of the letters X and O to show that the network is able to associate corrupted stimuli with their uncorrupted memories. We'll also show the network an uncorrupted copy of the unfamiliar letter F to see what memory it associates with an unfamiliar stimulus.

Using the HopfieldNets package, we can do this in Julia as follows:

{% highlight julia %}
using HopfieldNets
 
include(Pkg.dir("HopfieldNets", "demo", "letters.jl"))
 
patterns = hcat(X, O)
 
n = size(patterns, 1)
 
h = DiscreteHopfieldNet(n)
 
train!(h, patterns)
 
Xcorrupt = copy(X)
for i = 2:7
    Xcorrupt[i] = 1
end
 
Xrestored = associate!(h, Xcorrupt)
{% endhighlight %}

In the image below, I show what happens when we present X, O and F to the network after training it on the X and O patterns:

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2013/07/Results.jpg" 
       alt="Results" />
</center>

As you can see, the network perfectly recovers X and O from corrupted copies of those letters. In addition, the network associates F with an O, although the O is inverted relative to the O found in the training set. This kind of untrained memory emerging is common in Hopfield nets. To continue the analogy with LSH, you can think of the memories produced by the Hopfield net as hashes of the input, which have the property that similar inputs tend to produce similar outputs. In practice, you shouldn't use a Hopfield net to do LSH, because the computations involved are quite costly.

Hopefully this simple example has piqued your interest in Hopfield networks. If you'd like to learn more, you can read through the [code I wrote](https://github.com/johnmyleswhite/HopfieldNets.jl) or work through the very readable presentation of the theory of Hopfield networks in David Mackay's book on [Information Theory, Inference, and Learning Algorithms](http://www.inference.phy.cam.ac.uk/itila/book.html).
