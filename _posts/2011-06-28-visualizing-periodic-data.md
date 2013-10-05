---
title: Visualizing Periodic Data
author: John Myles White
layout: post
permalink: /notebook/2011/06/28/visualizing-periodic-data/
categories:
  - Programming
  - Statistics
---

Yesterday the Princeton machine learning reading group went through a paper by Tukey on ["Some graphic and semigraphic displays"](http://www.edwardtufte.com/tufte/tukey). One issue we talked about at length was Tukey's idiosyncratic approach to visualizing periodic data in a circular format to emphasize the connections between the "start" and the "end" of the data set.

Allison Chaney pointed out that many fields (for instance, environmental engineering) might want to consider using these circular displays to make periodic trends clear to the viewer. That inspired me to try plotting periodic weather data using both a standard x-y plane display and a polar coordinates display. The results are shown below in two videos that I've uploaded to Vimeo:

<center>
  <p><a href="http://vimeo.com/25716170">Visualizing Periodic Data: NYC Weather from 1995 to 2008</a> from <a href="http://vimeo.com/user698502">John Myles White</a> on <a href="http://vimeo.com">Vimeo</a>.</p>
</center>

<center>
  <p><a href="http://vimeo.com/25717081">Visualizing Periodic Data: NYC Weather from 1995 to 2008 (Take 2)</a> from <a href="http://vimeo.com/user698502">John Myles White</a> on <a href="http://vimeo.com">Vimeo</a>.</p>
</center>

There's a clear tradeoff that's being made when choosing between these two approaches: the polar coordinates plot, as promised, correctly connects the two "ends" of the data set. But it also makes it much harder to see the height of the graph at each point in time, so that the sinusoidal shape that can easily be seen in the x-y plane display is basically hidden in the polar coordinates display.

Since making these videos, it occurred to me that another potential visualization technique would be to project the data onto a cylinder, rather than a plane, and then progressively rotate the cylinder to reveal the time trend. This would allow heights to be seen properly, while emphasizing the periodicity. The problem with this cylindrical projection is that the entire data set is never fully visible at one time, but can only be seen by completing a full rotation of the data.

In his paper, Tukey describes one other approach: draw the periodic data twice so that the period is clearly visible. It wasn't clear to me how to do this without some numeric hacks in ggplot2, so I'll leave it to reader to search for Tukey's example in [the original paper](http://www.edwardtufte.com/tufte/tukey).
