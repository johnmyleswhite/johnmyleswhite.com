---
title: A Lot of Deaths are Partly Self-Induced
author: John Myles White
layout: post
permalink: /notebook/2009/12/11/a-lot-of-deaths-are-partly-self-induced/
categories:
  - Statistics
---

I'm a little surprised by [Andrew Gelman's post today](http://scienceblogs.com/appliedstatistics/2009/12/all_deaths_are_suicides.php
), doubting the wisdom of a passage from Gary Becker's work that reads:

<blockquote>
<p>According to the economic approach, therefore, most (if not all!) deaths are to some extent "suicides" in the sense that they could have been postponed if more resources had been invested in prolonging life.</p>
</blockquote>

I disagree with Gelman in finding this passage unimpressive or even slightly ridiculous. I think there is an important insight that Becker is trying to push on: we humans have far more control over our lives than we care to admit. I also think that not admitting the extent of our power to control our lives exculpates us for many of our failures, which is why it is one of the more popular approaches to dealing with the problems in our lives. As [La Rochefoucauld](http://en.wikiquote.org/wiki/François_de_La_Rochefoucauld) always pointed out, nothing trumps self-love.

And I don't think that emphasizing the unexpectedly large amount of control we have in our lives is just an idea that the economic approach has to offer us. I think you'd get the same idea out of reading Sartre's discussion of a young man choosing whether or not to fight in the French Resistance in [Existentialism is a Humanism.](http://www.marxists.org/reference/archive/sartre/works/exist/sartre.htm)

But, to focus on the thrust of Gelman's argument, my problems are really with the following passage:

<blockquote>
<p>My impression, though, is that people are dying all the time without wanting to do so, and Becker's argument seems pretty silly to me. To spell it out in a little more detail: Suppose a person is standing on the sidewalk and is run over by an out-of-control car. I don't see how you can call it suicidal of the pedestrian that the driver was not paying attention. Nor do I see it as suicidal if someone develops kidney cancer and dies, nor do I see it as suicidal if a kid is playing and falls out of a high window, or if someone in the Middle East is hit by a bomb while sitting in a school, etc. Beyond this, just as there used to be millions of people who smoked and died of cancer before people knew that smoking kills, I'm sure there are millions of people now doing something that they don't realize is dangerous.</p>
</blockquote>

The first section is worrisome, because it seems to be catering to the availability bias: we humans are almost certain to overestimate the number of deaths caused by car accidents, so, by referring to it without explicit numbers, we're likely to anchor ourselves on inaccurate quantities. In general, once any emotional topic comes up, I feel the need to find something more immutable than intuitions to trust in. Once a kid falls out of a window, rationality tends to follow them.

Now, I freely concede that all of the examples Gelman lists are not at all suicide-like, but what I worry about is the relative importance of those causes of death amid the frequent causes of death, at least in the U.S. I especially worry about this given the last two sentences of the above-cited passage: why focus on the "millions of people now doing something that they don't realize is dangerous?" Why not focus instead on the millions of people killing themselves by smoking, fully cognizant of their self-destruction all the while? What possible reason would you have for focusing on one or the other?

I'd say that, in general, you can focus on those who smoke if you want to prove Becker right, and that you can focus on those who don't if you want to prove him wrong. Either way, I am reasonably certain that you'll implicitly distort your internal estimates of the relative numbers.

With that in mind, I think the right approach is to get some plausible estimate from real data. Here's a simplistic start:

(1) Go to the CDC website and find a reasonably current list of the leading causes of death in the U.S. [here](http://www.cdc.gov/nchs/FASTATS/lcod.htm).

(2) Mark each item as a possible point for Becker's case. This is clearly debatable, but I'll take as granted that "heart disease" (over-eating and under-exercising), "chronic lower respiratory diseases" (smoking), "diabetes" (over-eating and under-exercising), and "influenza and pneumonia" (refusing vaccination) are sometimes self-induced. My own bias is to assume that they're usually self-induced, but that's pure intuition, so please don't trust that idea. In fact, what I'd really like is for you, dear reader, to prove me wrong with more elaborate statistics.

With these marked data points, I get a data set that looks like this:

<table class="table table-hover">
  <th>
    Cause of Death
  </th>
  <th>
    Deaths
  </th>
  <th>
    Partly Self-Induced
  </th>
  <tr>
    <td>
      Heart disease
    </td>
    <td>
      631636
    </td>
    <td>
      1
    </td>
  </tr>
  <tr>
    <td>
      Cancer
    </td>
    <td>
      559888
    </td>
    <td>
    </td>
  </tr>
  <tr>
    <td>
      Stroke (cerebrovascular diseases)
    </td>
    <td>
      137119
    </td>
    <td>
    </td>
  </tr>
  <tr>
    <td>
      Chronic lower respiratory diseases
    </td>
    <td>
      124583
    </td>
    <td>
      1
    </td>
  </tr>
  <tr>
    <td>
      Accidents (unintentional injuries)
    </td>
    <td>
      121599
    </td>
    <td>
    </td>
  </tr>
  <tr>
    <td>
      Diabetes
    </td>
    <td>
      72449
    </td>
    <td>
      1
    </td>
  </tr>
  <tr>
    <td>
      Alzheimer's disease
    </td>
    <td>
      72432
    </td>
    <td>
    </td>
  </tr>
  <tr>
    <td>
      Influenza and Pneumonia
    </td>
    <td>
      56326
    </td>
    <td>
      1
    </td>
  </tr>
  <tr>
    <td>
      Nephritis, nephrotic syndrome, and nephrosis
    </td>
    <td>
      45344
    </td>
    <td>
    </td>
  </tr>
  <tr>
    <td>
      Septicemia
    </td>
    <td>
      34234
    </td>
    <td>
    </td>
  </tr>
</table>

Copying and pasting the resulting table and running two lines of R code lets me avoid the relevant mental arithmetic:

{% highlight r %}
death.data <- read.csv('death_data.csv', header = TRUE, sep = '\t')

with(subset(death.data, Partly.Self.Induced == 1), sum(Deaths)) / with(death.data, sum(Deaths))
{% endhighlight %}

And I get 48% of all deaths as being self-induced. That's definitely not "almost all", but plainly a close call for "most." Obviously this number is dubious at best: my point is only that so many cognitive biases operate in thinking about this sort of issue that you have to use numbers as a form of mental hygiene. Obviously pushing this issue into the realm of statistics only endangers my claims, since Gelman is so much more capable than I am as a statistician, but I think my general point is accurate: whether a given person thinks the numbers support Becker or disprove him says more about that person's general outlook on life than about the actual question at hand.
