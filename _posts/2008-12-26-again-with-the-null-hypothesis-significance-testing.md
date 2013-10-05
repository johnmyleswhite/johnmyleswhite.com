---
title: Again with the Null Hypothesis Significance Testing
author: John Myles White
layout: post
permalink: /notebook/2008/12/26/again-with-the-null-hypothesis-significance-testing/
categories:
  - Statistics
---

As I was finishing reading "The Cult of Statistical Significance" yesterday, the following passage struck me as particularly important:

<blockquote>
<p>Rothman computed a <em>p</em>-value function -- a continuous function of *p*-values mapped against a range of effect sizes. The range of effect sizes was here again measured by the relative risk ratio and includes both beneficial and nonbeneficial effects. He shows that another hypothesis, a fantastically beneficial risk ratio, RR = 4.1, shares the same <em>p</em>-value, .14, as the null, RR = 1.0 (2002, 125). This is common in medicine and all the sciences. To think that *p*-values have a 1-to-1 correspondence with a unique risk ratio is to ignore the symmetry of the <em>p</em>-function.</p>

<small>Stephen T. Ziliak and Deirdre N. McCloskey : The Cult of Statistical Significance : On Drugs, Disability and Death</small>
</blockquote>

I wish the symmetry of the distributions used for testing significance, especially the *t* distribution, would be emphasized to students during their introduction to statistics. We generally test distributions so that the *t*-value comparison is strongly positive to see whether we can reject the null hypothesis of zero difference between the means for some two sets of observations. But it is always possible to test another null hypothesis, in which the difference between the means for the two groups is much larger than the difference we observed, that we will also always fail to reject every time that we fail to reject the primary null hypothesis of zero difference. Yet we never test this hypothesis -- despite their being no good reason for this mathematically. The only justification is an implicit Bayesian prior in defense of the null hypothesis rather than its dopplegänger hypothesis in which the difference is much larger than we have seen in practice. Is this implicit underweighting of the alternative null hypothesis really sound? That is an empirical question that is, unfortunately, not likely to be answered soon, but it suggests that conventional statistical practice may consistently underestimate the effects being examined using significance testing.

Of course, this problem is itself tied to the erroneous conflation of a failure to reject the null hypothesis with its acceptance -- with the result that statistically insignificant effects are treated as non-existent, rather than inconclusively determined by the data at hand. Statistically insignificant differences tend not to be tested empirically a second time, so that it is hard to know how often they are really larger than our first experiments suggested.
