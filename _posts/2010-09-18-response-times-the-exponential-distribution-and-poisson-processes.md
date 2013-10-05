---
title: Response Times, The Exponential Distribution and Poisson Processes
author: John Myles White
layout: post
permalink: /notebook/2010/09/18/response-times-the-exponential-distribution-and-poisson-processes/
categories:
  - Psychology
  - Statistics
---

I'm currently reading [Luce's "Response Times"](http://www.amazon.com/Response-Times-Elementary-Organization-Psychology/dp/0195070011/ref=sr_1_1?s=books&ie=UTF8&qid=1284837406&sr=1-1). If you don't know anything about response times, they are very easily defined: a response time is the length of time it takes a person to respond to a simple request, measured from the moment when the request is made to the moment when the person's response is recorded. In principle, you can measure response times when asking people to indicate that they've heard a tone as easily as you can measure them when you've asked people to solve a problem in calculus. In practice, response times are most easily analyzed when the task a person is performing is so short that the person completing it can usually be assumed not to be performing other tasks simultaneously.

To convince non-psychologists that response times are a quantity worth measuring, I'll give an example from my own work that I suspect is widely generalizable. I consistently find that the quality of data I can extract from Mechanical Turk HIT's grows considerably if I simply weight each person's responses by a function of their time to complete the HIT. For one recent task, the histogram of task completion times looked like the following:

<center>
  <img src="http://www.johnmyleswhite.com/notebook/wp-content/uploads/2010/09/rts.png" alt="rts.png" />
</center>

As you can obviously see, there are two noticeable clusters of participants for this HIT: a substantial chunk who spent shockingly little time on the task and a large majority of people who spent a longer and more variable period of time performing the task. In this case, completely excluding those who completed this specific task within ten seconds improved the results of my later analyses considerably at virtually no cost to myself. Other approaches, like using k-means clustering, mixture modeling or simple quantile cut-offs, can be equally useful. **In general, response times are a useful proxy for the amount of effort a person puts into a task.**

Hopefully that example convinces you that reaction times are worth studying carefully. If you're interested, I strongly recommend Luce's book as an introduction to the use of response times to infer information about the mental state of specific human beings and about the global architecture of the human mind in general. It's quite long, so many people would probably gain the most from only reading the first few chapters.

In the rest of this post, I'm going to discuss a detail about the measurement of response times that I found particularly beautiful and insightful. Specifically, I was struck by a point that Luce raises in the second chapter of his book: the proper measurement of response times requires the use of an experimental design that employs the exponential distribution to produce a Poisson process of stimuli. That's probably opaque to most readers, so let me unpack it:

* **Our Objective**: We want to estimate the speed with which a normal person can respond to a stimulus.
* **Our High-Level Method**: We present subjects with a train of simple sensory stimuli, like 10 dB tones, and ask them to respond as quickly as possible to each by pressing a button. We record the time from the onset of the tone to the moment when they press the button.
* **Our Challenge**: The length of time before a stimulus is presented must be randomly distributed, so that subjects cannot adopt a fixed strategy for timing their responses. More importantly, we must insure that the mere act of waiting for a stimulus is not itself informative: the subject should not be able to notice that the arrival of a stimulus is becoming progressively more or less likely as the time they've been waiting grows. This means that a naive sampling distribution for waiting times before presenting a stimulus, like the uniform distribution, is simply not acceptable.
* **Our Solution**: We calculate a measure of the informativeness of each moment of waiting: the measure we use is called the [hazard rate](http://en.wikipedia.org/wiki/Hazard_rate) and can be calculated for any probability distribution. Using this metric of informativeness, we simply need to find a probability distribution that has a constant hazard rate. The distribution we arrive at is [the exponential distribution](http://en.wikipedia.org/wiki/Exponential_distribution). A train of stimuli separated by exponentially distributed waiting periods is called a [Poisson process](http://en.wikipedia.org/wiki/Poisson_process). It allows us to treat all of the instants within a single trial's waiting period as comparable.

For me, this was a revelatory example that finally made the exponential distribution's relevance clear. Understanding this point made me understood intuitively, rather than simply algebraically, why Luce says this in the first chapter:

<blockquote>
<p>[We] see that the answer is [...] the exponential distribution. This may come as a bit of a surprise since, after all, the largest value of the exponential value is at 0 -- instantaneous repetition -- which must tend to produce clustering if the process is repeated in time. This is, indeed, the case, as can either be seen, as in Figure 1.3, or heard if such a process is made audible in time -- it seems to sputter. To neither the eye nor the ear does it seem uniform in time, but that is an illusion, confusing what one sees or hears with the underlying rate of these events. The tendency for the event to occur is constant. Such a process is called <em>Poisson</em> -- more of it in Chapters 4 and 9. It is the basic concept of constant randomness in time -- that is, the unbounded, ordered analogue of a uniform density on a finite, unordered interval.</p>
</blockquote>
