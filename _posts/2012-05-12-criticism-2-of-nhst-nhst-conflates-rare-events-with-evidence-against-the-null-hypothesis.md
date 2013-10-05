---
title: "Criticism 2 of NHST: NHST Conflates Rare Events with Evidence Against the Null Hypothesis"
author: John Myles White
layout: post
permalink: /notebook/2012/05/12/criticism-2-of-nhst-nhst-conflates-rare-events-with-evidence-against-the-null-hypothesis/
categories:
  - Academia
  - Economics
  - Psychology
  - Science
  - Statistics
---

### Introduction

This is my second post in a series describing the weaknesses of the NHST paradigm. [In the first post](http://www.johnmyleswhite.com/notebook/2012/05/10/criticism-1-of-nhst-good-tools-for-individual-researchers-are-not-good-tools-for-research-communities/), I argued that NHST is a dangerous tool for a community of researchers because p-values cannot be interpreted properly without perfect knowledge of the research practices of other scientists -- knowledge that we cannot hope to attain.

In this post, I will switch gears and focus on a weakness of NHST that afflicts individual researchers as severely as it afflicts communities of researchers. This post focuses on the concern that NHST is not robust to "absolutely rare" events, by which I mean events that have low probability under all possible hypotheses. NHST erroneously treats such "absolutely rare" events as evidence against the null hypothesis. In practice, this means that many researchers will treat such "absolutely rare" events as evidence in support of the alternative hypothesis, even when the null hypothesis assigns higher probability to the observed data than the alternative hypothesis and should therefore be considered the superior model.

To explain this concern using a detailed example, I will borrow an idea from Jacob Cohen. Before I do that, I need to outline a version of NHST that I believe is an accurate description of the way many practicing scientists actually use NHST.

In this formulation, the scientist observes an event; then posits a model, called the null hypothesis, that accounts for this and similar events using only chance mechanisms; and then estimates the probability of observing this sort of event under the null hypothesis. If the event and its kind are assigned low probability, the null hypothesis is rejected. For many scientists, this rejection is taken as evidence in favor of their preferred hypothesis, which we call the alternative hypothesis.

### Cohen's Example: Americans and Members of Congress

In our example, we will imagine meeting a new person named John Smith. We entertain two hypotheses about him: one, the null hypothesis, asserts that John Smith is an American. The other, the alternative hypothesis, asserts that John Smith is not an American. Because these hypotheses are mutually exclusive, it seems acceptable to say that any evidence against the null hypothesis should constitute evidence in support of the alternative hypothesis. We will demonstrate that p-values cannot constitute such evidence.

We do this by creating a simple null model: under the null hypothesis, the probability that John Smith is a Member of Congress is roughly $535 / 311,000,000$ which is close to two in a million. In short, John Smith being a Member of Congress is a very improbable event under the null hypothesis.

Under the alternative hypothesis, the probability that John Smith is a Member of Congress is `0`, because non-Americans cannot serve in Congress. This is an uncommon virtue in probabilistic models, because the existence of an event with zero probability means that the alternative hypothesis is actually falsifiable.

Now suppose that we do not know John Smith's citizenship, but we do find out that he is a Member of Congress. Using NHST, we will perversely reject the null hypothesis that John is an American because Americans are very rarely Members of Congress. If we continue on from this rejection of the null to an acceptance of the alternative hypothesis, we will conclude that John Smith is not an American.

This is troubling, because a simple calculation using Bayes' Theorem will demonstrate that the probability that John Smith is an American given that he is a Member of Congress is `1`: we are absolutely certain that John Smith is an American. Yet NHST would have us reject this absolutely certain conclusion. For those who harbor an inveterate suspicion of Bayes' Theorem, we can note that the likelihood ratio in this example is infinite in support of the null hypothesis and that a significance test of this ratio leads us to fail to reject the null hypothesis. (See footnote)

### How Can NHST Make Such An Obvious Mistake?

For many readers, I suspect that this example will seem too outlandish to be trusted: it seems hard to believe that the basic machinery of NHST could be so fragile as to allow an example like ours to break it. Such readers are right to suspect that some trickery is required to power our thought experiment: we must assume that we have observed data that is "absolutely rare", in the sense that the probability that a person is a Member of Congress, across both the null hypothesis and the alternative hypothesis, is only one in ten million. As such, when applied to randomly selected human beings, our hypothesis test will perform well: we will make the mistake I have highlighted only one in ten million times. As a bet, NHST is safe: indeed, there are few safer bets if we only select randomly among Americans while using a test with such a low false positive rate. But when a rare event does occur, NHST will produce erroneous results. In other words, NHST treats rare events interchangeably with evidence against the null hypothesis, even though this equivalence is not defensible when the data is viewed from another perspective. Moreover, if we begin to select randomly from the true population of the Earth, our use of a procedure with such an incredibly low false positive rate is actually a serious error, because our method has very low power to detect non-Americans -- even though they are the vast majority of all human beings. For those who believe that the null hypothesis is almost always false in research studies, the use of underpowered methods is particularly troubling.

Sadly, unambiguous examples like this one are harder to construct using the types of data typically employed in the modern sciences, because our hypotheses and measurements are typically continuous rather than binary and are rarely absolutely falsifiable. And because we typically lack verifiable base rate information, we cannot expect to use Bayes' Theorem to calculate the relative probability of our hypotheses without subjective decisions about the a priori plausibility of our hypotheses. For some readers, this may make our example seem misleading: NHST only looks bad when clearly superior methods are available, which they seldom are.

But I think that this example nevertheless reveals a deep weakness with NHST: absolutely rare events will happen. A method that rejects the null hypothesis when rare events occur without even testing whether the alternative hypothesis is plausible seems very questionable to me, especially when we use a liberal threshold like $p < 0.05$. Because events only need to be very weakly rare to merit rejection under conventional NHST standards, the sort of hidden multiple testing I discussed in the last post may insure that rare events occur frequently enough to be reported much more frequently in the literature than our nominal false positive rate suggests. If a rare event only needs to be $p = 0.05$ level rare to be publishable, then one only needs to conduct twenty studies in a row to produce such a rare event. Surely people can work hard enough to produce that sort of rarity when their careers depend upon it.

### How Can We Do Better?

What lesson should we take away from this bizarre example in which NHST leads us to reject a hypothesis that the data incontrovertibly supports? In my mind, the lesson that we should take away is that we cannot hope to learn about the relative plausibility of two hypotheses without assessing both hypotheses explicitly. We should not fault the null hypothesis for failing to predict data that the alternative hypothesis would not have predicted any better. Our interest as scientists should always lie in creating and selecting models with superior predictive power: we learn little to nothing by defeating a hypothesis that may not have been given a fair chance because of the quirky data being used to test it.

NHST does not satisfy the demand to evaluate both hypotheses explicitly, because it does all calculations using only the null hypothesis while entirely ignoring the alternative hypothesis. This is particularly troubling when so many practicing scientists treat a low p-value as evidence in support of the alternative hypothesis, even though this conclusion cannot generally be supported and was not part of Fisher's original intention when introducing NHST. In short, NHST too often leads us to reject true hypotheses and too easily leads us to transform our rejection of true hypotheses into an acceptance of inferior hypotheses.

### References

Cohen, J. (1994), 'The Earth is Round (p < .05)', American Psychologist [Ungated Copy](http://ist-socrates.berkeley.edu/~maccoun/PP279_Cohen1.pdf)

### Footnotes

*In passing, I note that it should trouble readers that two different types of NHST give opposite answers. This is just one example of what Bayesians would call the incoherence of NHST as a paradigm.*
