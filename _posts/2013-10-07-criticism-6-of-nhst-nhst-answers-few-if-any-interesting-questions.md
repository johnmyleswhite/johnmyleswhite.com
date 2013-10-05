---
title: "Criticism 6 of NHST: NHST Answers Few, if Any, Interesting Questions"
author: John Myles White
layout: post
categories:
  - Academia
  - Economics
  - Psychology
  - Statistics
---

*This post is a rough draft of an approach to formalizing the problems with NHST that I've been thinking about for some time. In order to make it clear why I am so opposed to the use of NHST in psychology and the other social sciences, I attempt to use Pearl's causal graphs to encode the structure of social scientific theories. I then articulate both my current beliefs about social science as properties of the graph (namely completeness) and the types of errors in terms of properties of the graph.*

Judea Pearl has done amazing work demonstrating that causal graphs provide a rigorous mathematical framework for discussing causality.

If X is directly causally related to Y, we place an edge between the nodes that represent X and Y in the graph.

We can reframe Meehl's concerns in terms of this diagrams quite easily:

* An ontology is an enumeration of which nodes should be present in the graph. We might, for example, think that there should be nodes for the Big Five:
	* Conscientiousness
* The grosstest classification of the relationships between

Meehl and Lykken spoke of a cruft factor in psychology, in which everything is correlated with everything. In our terms, this means

What do we use NHST for? We use it to demonstrate.

* NHST is used to argue that an edge exists
* Estimation assigns a weight to the edge

What type of errors occur:

* Type I errors: edges are added that don't belong
* Type II errors: edges aren't added that do belong
* Type M errors: the weights assigned to edges are of the wrong size
* Type S errors: the weights assigned to edges are of the wrong size and predict effects in the opposite direction of what truly happens

Using the language of graph theory

Although it is uncharitable to say so, I believe part of the interest in the null hypothesis in psychology comes from a lack of familiarity with quantiative precision. Let us be clear what an absent edge (or an effect of exactly zero) means.

One way to define this is to say that, for every microscope proposed effect size, you believe the true effect is smaller than that. To me, this is madness -- how could you ever hope to demonstrate that the effect of X on Y is smaller than 10e-1000? At some point, you have to concede that the effect can only be treated _as if it were zero_. But the trouble is that it's not at all clear where the cut-off should be. In my mind, many of the famed effects published in psychology are probably small enough as to be meaningless -- yet people make widely successful careers out of them.
